# Day 11

## Week 3

This day has the heaviest workload for the week so do not be discouraged if you can not
finish it by the end of the day.

Write the answers to each question inside `assignments/day11/answers.md`.

## Database

Start the day by finishing the `database.js` TODOs.

## Test Results

Now we will add test results to each build in our jenkins pipeline that will keep track
of the code coverage for each build.

Start by modifying the `npm run unit:test` script so that it collects coverage for the
tests.

When you run the command you should see a `coverage` directory inside your `game-api`
directory that contains the code coverage results for your unit tests.

Then install the `Clover` Jenkins plugin.

Then add the following:

```Jenkinsfile
step([
  $class: 'CloverPublisher',
  cloverReportDir: 'coverage',
  cloverReportFileName: 'clover.xml',
  healthyTarget: [methodCoverage: 80, conditionalCoverage: 80, statementCoverage: 80],
  unhealthyTarget: [methodCoverage: 50, conditionalCoverage: 50, statementCoverage: 50],
  failingTarget: [methodCoverage: 0, conditionalCoverage: 0, statementCoverage: 0]
])
```

You are done when Jenkins starts generating and displaying the coverage results for
each build.

## Server

Before you start implementing the API and Capacity tests you should answer the TODO
inside `server.js`, understanding how writing asynchronous code using callbacks in
NodeJS vill help you understand how the API and capacity tests below work.

## API Tests

Now it is time to create tests that run against our API. They work by calling a 
running instance of the API like you have been doing using `curl` or `postman`.

We will use the `request` npm package to call our API.

`server.lib-test.js`:
```javascript
const request = require('request');

const playGame = (url, done) => {
  request.post(url + '/start', function(error, response, body) {
    if (error) {
      done.fail(error);
      return;
    }
    console.log('start: ' + response['statusCode'] + ' - ' + body);
    guessUntilGameIsOver(url, 12, done);
  });
};

// Recursive function that makes a guess until the game is over
// or the maxGuesses is reached.
const guessUntilGameIsOver = (url, maxGuesses, done) => {
  if (maxGuesses === 0) {
    done.fail('the game is never over');
    return;
  }
  request.get(url + '/state', function(error, response, body) {
    if (error) {
      done.fail(error);
      return;
    }
    console.log('state: ' + response['statusCode'] + ' - ' + body);
    if (JSON.parse(body).gameOver) {
      done();
      return;
    }
    const possibilities = ['guess21OrUnder', 'guessOver21'];
    const guess = possibilities[Math.floor(Math.random() * 2)];
    request.post(url + '/' + guess, function(error, response, body) {
      if (error) {
        done.fail(error);
        return;
      }
      console.log('guess: ' + response['statusCode'] + ' - ' + body);
      guessUntilGameIsOver(url, maxGuesses - 1, done);
    });
  });
};
                              
module.exports = {
  playGame: playGame,
  guessUntilGameIsOver: guessUntilGameIsOver,
};
```

`server.api-test.js`:
```javascript
const helper = require('./server.lib-test.js');

const timeout = 30000;

// TODO what does the done parameter do?
test('play a game', function(done) {
  helper.playGame(process.env.API_URL, done);
}, timeout);
```

Create a script in your `package.json` named `test:api` that runs Jest on all files 
ending in `.api-test.js`.
                                                        
Try starting your API using docker-compose locally and running your API tests:\
`export API_URL=http://localhost:3000 && npm run test:api`

If you have not played the game yourself using `curl` or `postman`, you will probably
find some errors in your API. This is the reason for creating API tests, to verify that
the API works as intended without having to call it manually before each release.

## Capacity Tests

Now it is time to create tests that run against our API.

Using the helper library we created for the API but running it multiple times.

`server.capacity-test.js`:
```javascript
const helper = require('./server.lib-test.js');

const timeout = 30000;
const gameCount = 10;

const playGames = (url, count, done) => {
  if (count === 0) {
    done();
    return;
  }

  // Creating a callback that works the same way as done in Jest.
  const playGameCallback = () => {
    playGames(url, count - 1, done);
  };
  playGameCallback.fail = done.fail;

  helper.playGame(url, playGameCallback);
};

test('play ' + gameCount + ' games within ' + (timeout / 1000) + ' seconds', function(done) {
  playGames(process.env.API_URL, gameCount, done);
}, timeout);
```

Create a script in your `package.json` named `test:capacity` that runs Jest on all files 
ending in `.capacity-test.js`.
                                                        
Try starting your API using docker-compose locally and running your API tests:\
`export API_URL=http://localhost:3000 && npm run test:capacity`

Capacity tests are useful to verify that each release is performing as we expect, in
our case we have configured the test so that it will verify that we can play 10 games
within 30 seconds (took around 15-20 seconds), so if we introduce changes to the code
later on that break the test we know down to a commit which changes reduced the
performance.

## The Pipeline

Now that we have created our API and capacity tests it's time to integrate them into the
pipeline. Currently our pipeline is only 2 stages:\
Commit => Deployment\
Our goal is to add the API and Capacity stages, like so:\
Commit => API Test => Capacity Test => Deployment

We want our tests to run against an API as close to identical to our production environment
as possible. Luckily Terraform makes it easy for us create identical infrastructures in
the cloud using our `.tf` file.

But we have a problem, as is our `infrastructure.tf` file is configured to create a
security group named `GameSecurityGroup` and AWS requires our security groups to be
unique. To fix this we will add a parameter to our terraform file:
```hcl-terraform
# Top of file
variable "environment" {
  type = "string"
}

# Usages
name   = "GameSecurityGroup_${var.environment}"

Name = "GameServer_${var.environment}"
```

To use the parameter without requiring user intervention, add this:
```bash
terraform destroy -auto-approve -var environment=production || exit 1
terraform apply -auto-approve -var environment=production || exit 1
```
to your Jenkins deployment project.

The next time Jenkins deploys your project the instance and security group should have
a `_production` suffix.

### API Test Job

Now create a new Jenkins job identical to the deployment job except:
- It should run Terraform from `/var/lib/jenkins/terraform/hgop/apitest`
- It should have `apitest` as the Terraform environment parameter value.
- It should run the API tests against the new `apitest` instance.
- It should destroy the `apitest` instance at the end.

Add
```Jenkinsfile
build job: 'job-name-api-test', parameters: [[$class: 'StringParameterValue', name: 'GIT_COMMIT', value: "${git.GIT_COMMIT}"]]
```
to your Jenkinsfile.

Once you are done the API tests should be run against an `apitest` instance.

### Capacity Test Job

Now create a new Jenkins job identical to the deployment job except:
- It should run Terraform from `/var/lib/jenkins/terraform/hgop/capacitytest`
- It should have `capacitytest` as the Terraform environment parameter value.
- It should run the capacity tests against the new `capacitytest` instance.
- It should destroy the `capacitytest` instance at the end.

Add
```Jenkinsfile
build job: 'job-name-capacity-test', parameters: [[$class: 'StringParameterValue', name: 'GIT_COMMIT', value: "${git.GIT_COMMIT}"]]
```
to your Jenkinsfile.

Once you are done the capacity tests should be run against an `capacitytest` instance.

## Handin

You should store all the source files in your repository:

```bash
├── game-api
│   ├── .eslintrc.json
│   ├── app.js
│   ├── server.js
│   ├── server.lib-test.js
│   ├── server.api-test.js
│   ├── server.capacity-test.js
│   ├── config.js
│   ├── random.js
│   ├── random.unit-test.js
│   ├── deck.js
│   ├── deck.unit-test.js
│   ├── dealer.js
│   ├── dealer.unit-test.js
│   ├── lucky21.js
│   ├── lucky21.unit-test.js
│   ├── inject.js
│   ├── context.js
│   ├── database.js
│   ├── Dockerfile
│   └── package.json
├── assignments
│   ├── day01
│   │   └── answers.md
│   ├── day02
│   │   └── answers.md
│   └── day11
│       └── answers.md
├── scripts
│   ├── initialize_game_api_instance.sh
│   ├── verify_environment.sh
│   ├── docker_compose_up.sh
│   ├── docker_build.sh
│   ├── docker_push.sh
│   ├── sync_session.sh
│   └── deploy.sh
├── docker-compose.yml
├── infrastructure.tf
├── Jenkinsfile
└── README.md
```

They must be placed at these location to get full marks.
