This [repo contains everything you need to get started](https://github.com/howdyai/botkit-starter-slack) with the Slack Events API.

### Instant Start
One click deploy your project via your bot's hosting tab. [Click on your bots inventory](https://botkit.ai/app), choose a bot, and then `Integrations`->`Hosting` for more information.


### Get Started

Clone this repository:

`git clone https://github.com/howdyai/botkit-starter-slack.git`

Install dependencies, including [Botkit](https://github.com/howdyai/botkit):

```
cd botkit-starter-slack
npm install
```

Set up a new Slack application via the Slack developer portal. This is a multi-step process, but only takes a few minutes. [Read this step-by-step guide](https://github.com/howdyai/botkit/blob/master/docs/slack-events-api.md) to make sure everything is set up.

Next, get a Botkit Studio token [from your Botkit developer account](https://studio.botkit.ai/)

Now, run your bot from the command line with your new tokens:

`clientId=<MY SLACK TOKEN> clientSecret=<my client secret> PORT=<3000> studio_token=<MY BOTKIT STUDIO TOKEN> node bot.js`

Now, visit your new bot's login page: http://localhost:3000/login

Once successfully logged in, your bot should connect to Slack AND Botkit Studio and leap into action!

Continue your journey to becoming a champion botmaster by [reading the Botkit Studio SDK documentation here.](https://github.com/howdyai/botkit/blob/master/readme-studio.md)