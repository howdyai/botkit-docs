This [repo contains everything you need to get started](https://github.com/howdyai/botkit-starter-facebook) with Facebook Messenger.

### Instant Start
One click deploy your project via your bot's hosting tab. [Click on your bots inventory](https://botkit.ai/app), choose a bot, and then `Integrations`->`Hosting` for more information.

### Get Started

Clone this repository:

`git clone https://github.com/howdyai/botkit-starter-facebook.git`

Install dependencies, including [Botkit](https://github.com/howdyai/botkit):

```
cd botkit-starter-facebook
npm install
```

Get a Facebook access tokens [as described here](https://github.com/howdyai/botkit/blob/master/readme-facebook.md#getting-started)

Get a Botkit Studio token [from your Botkit developer account](https://studio.botkit.ai/)

Run your bot from the command line with your new tokens:

`page_token=<page access token> verify_token=<webhook verification token> studio_token=<botkit studio token> node .`

Facebook requires your application be available at an SSL-enabled endpoint. To expose an endpoint during development, we recommend using [localtunnel.me](http://localtunnel.me) or [ngrok](http://ngrok.io), either of which can be used to temporarily expose your bot to Facebook. Once stable and published to the real internet, use nginx or another web server to provide an SSL-powered front end to your bot application.

Continue your journey to becoming a champion botmaster by [reading the Botkit Studio SDK documentation here.](https://github.com/howdyai/botkit/readme-studio.md)
