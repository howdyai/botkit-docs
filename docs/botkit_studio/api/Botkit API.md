Using the Botkit API you can extend the functionality of your Botkit bots.

* [Receiving Messages](https://github.com/howdyai/botkit#receiving-messages) - Botkit bots receive messages through a system of specialized event handlers. Handlers can be set up to respond to specific types of messages, or to messages that match a given keyword or pattern. 
* [Sending Messages](https://github.com/howdyai/botkit#sending-messages) - Bots have to send messages to deliver information and present an interface for their functionality. Botkit bots can send messages in several different ways, depending on the type and number of messages that will be sent.
* [Middleware](https://github.com/howdyai/botkit#middleware) - The functionality of Botkit can be extended using middleware functions. These functions can plugin to the core bot running processes at several useful places and make changes to both a bot's configuration and the incoming or outgoing message.

### Advanced Functionality

* [Storing Information](https://github.com/howdyai/botkit#storing-information) - Botkit has a built in storage system used to keep data on behalf of users and teams between sessions. Botkit uses this system automatically when storing information for Slack Button applications;
* [Writing your own storage module](https://github.com/howdyai/botkit#writing-your-own-storage-module) - If you want to use a database or do something else with your data, you can write your own storage module and pass it in.
* [Writing your own logging module](https://github.com/howdyai/botkit#writing-your-own-logging-module) - By default, your bot will log to the standard JavaScript console object available in Node.js. This will synchronously print logging messages to stdout of the running process.
* [Use Botkit with an Express web server](https://github.com/howdyai/botkit#use-botkit-with-an-express-web-server) - Instead of controller.setupWebserver(), it is possible to use a different web server to manage authentication flows, as well as serving web pages.

More about The latest Botkit API [documents can be found on the public GitHub.](https://github.com/howdyai/botkit#developing-with-botkit)
Last Updated: 12/06/16
