# Please look the changes made in day 8 for the jenkins deployment job script

# You should add a production flag in your Dockerfile: `RUN npm install --production`

# Game won't be playable until you add
```javascript
getState: (game) => {
  return {
    cards: game.state.cards,
    cardsValue: game.getCardsValue(game),
    card: game.state.card,
    cardValue: game.getCardValue(game),
    total: game.getTotal(game),
    gameOver: game.isGameOver(game),
    playerWon: game.playerWon(game),
  };
},
```
to your lucky21 file.

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
 - URL to the Jenkins instance running on port 8080.
 - Any additional information about the project you want us to take into account.

The Jenkins deploy job should output the ip the game API is running on to the console.\
We will use this ip to check if the API is running.
