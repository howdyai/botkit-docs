This [repo contains everything you need to get started](https://github.com/howdyai/botkit-starter-slackrtm) with the Slack RTM API.

---

**Before you begin**: we highly recommend that new bot developers [start with our new starter kit which uses the more modern Slack Events API](https://github.com/howdyai/botkit-starter-slack), as is thus more feature rich and easier to manage.

---

### Instant Start
One click deploy your project via your bot's hosting tab. [Click on your bots inventory](https://botkit.ai/app), choose a bot, and then `Integrations`->`Hosting` for more information.


### Get Started

Clone this repository:

`git clone https://github.com/howdyai/botkit-starter-slackrtm.git`

Install dependencies, including [Botkit](https://github.com/howdyai/botkit):

```
cd botkit-starter-slackrtm
npm install
```

Get a Slack bot token [from your Slack team](https://my.slack.com/apps/new/A0F7YS25R-bots)

Get a Botkit Studio token [from your Botkit developer account](https://studio.botkit.ai/)

Run your bot from the command line with your new tokens:

`token=<slack token> studio_token=<botkit studio token> node .`

Your bot should connect to Slack AND Botkit Studio and leap into action!

Continue your journey to becoming a champion botmaster by [reading the Botkit Studio SDK documentation here.](https://github.com/howdyai/botkit/blob/talkabot/readme-studio.md)