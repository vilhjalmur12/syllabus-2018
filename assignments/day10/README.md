# Please look the changes made in day 8 for the jenkins deployment job script

# Week 2 - Assignment

**Turn in your GitHub repository URL into Canvas solo or in pairs (if the Github
repository is private please add kthorri and ironpeak as
collaborators)**

**Create a Jenkins user for us, and turn it's username and password in as a
comment with the URL**

You should store all the source files in your repository:

```bash
├── game-api
│   ├── .eslintrc.json
│   ├── app.js
│   ├── server.js
│   ├── config.js
│   ├── random.js
│   ├── random.unit-test.js
│   ├── deck.js
│   ├── deck.unit-test.js
│   ├── dealer.js
│   ├── dealer.unit-test.js
│   ├── lucky21.js
│   ├── lucky21.unit-test.js
│   ├── inject.js
│   ├── context.js
│   ├── database.js
│   ├── Dockerfile
│   └── package.json
├── assignments
│   ├── day01
│   │   └── answers.md
│   └── day02
│       └── answers.md
├── scripts
│   ├── initialize_game_api_instance.sh
│   ├── verify_environment.sh
│   ├── docker_compose_up.sh
│   ├── docker_build.sh
│   ├── docker_push.sh
│   ├── sync_session.sh
│   └── deploy.sh
├── docker-compose.yml
├── infrastructure.tf
├── Jenkinsfile
└── README.md
```

They must be placed at these location to get full marks.

Add a `README.md` file, it should include:
 - URL to the instance running the API running on port 3000.
 - URL to the Jenkins instance running on port 8080.
 - Any additional information about the project you want us to take into account.
