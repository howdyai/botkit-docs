# Botkit: Building Blocks for Building Bots

[![npm](https://img.shields.io/npm/v/botkit.svg)](https://www.npmjs.com/package/botkit)
[![David](https://img.shields.io/david/howdyai/botkit.svg)](https://david-dm.org/howdyai/botkit)
[![npm](https://img.shields.io/npm/l/botkit.svg)](https://spdx.org/licenses/MIT)
[![bitHound Overall Score](https://www.bithound.io/github/howdyai/botkit/badges/score.svg)](https://www.bithound.io/github/howdyai/botkit)

---

"Bot" is a word we use to describe software that sends and receives messages to present a user interface, rather than (or frequently, alongside) a traditional graphical UI. Many bots exist alongside real human users in chatrooms, and can take input in informal or "natural" language, which sometimes allows them to take the form of robotic coworkers or agents.

 Under their robotic skin, bots are actually connected web applications, and share many of the same components as websites or apps built for other platforms.

 The goal of Botkit is to provide the core set of features and tools necessary to operate a bot on any messaging system, and allow those features to be customized and applied to specific tasks through a semantic, clearly documented programming library.

Out of the box, Botkit bots include:

* Conversation management system
* Platform connectors for popular messaging apps
* Easy to extend plugin system
* Integration with top NLP tools
* Content management
* Analytics
* A self-contained webserver

# Install Botkit

[Read our full guide to getting started with Botkit](/getstarted.html).

# Build Your Bot

Building a bot should feel cool, and not too technically complicated.

Botkit handles all the nitty gritty details like API calls, session management and authentication,
allowing you to focus on building COOL FEATURES for your bot.

The toolkit is designed to provide meaningful building blocks for creating conversational user interfaces - with functions like `hears()`, `ask()`, and `reply()` that do what they say they do.

### Hearing Keywords

Most bots do their thing by listening for keywords, phrases or patterns in messages from users. Botkit has a special event handler called `hears()` that makes it easy to configure your bot to listen for this type of trigger.

```javascript
controller.hears(['string','pattern .*',new RegExp('.*','i')],'message_received,other_event',function(bot, message) {

  // do something!
  bot.reply(message, 'I heard a message.')

});
```

[Read more about hearing things &rsaquo;](docs/readme.md#matching-patterns-and-keywords-with-hears)

### Responding to Events

Bots can respond to non-verbal events as well, like when a new user joins a channel, a file gets uploaded, or a button gets clicked. These events are handled using an event handling pattern that should look familiar. Most events in Botkit can be replied to like normal messages.

```javascript
controller.on('channel_join', function(bot, message) {

  bot.reply(message,'Welcome to the channel!');

});
```

[See a full list of events and more information about handling them &rsaquo;](docs/readme.md#responding-to-events)

### Middleware

In addition to taking direct action in response to a certain message or type of event, Botkit can also take passive action on messages as they move through the application using middlewares. Middleware functions work by changing messages, adding new fields, firing alternate events, and modifying or overriding the behavior of Botkit's core features.

Middleware can be used to adjust how Botkit receives, processes, and sends messages. [Here is a list of available middleware endpoints](docs/readme-pipeline.md).

```javascript
// Log every message recieved
controller.middleware.receive.use(function(bot, message, next) {

  // log it
  console.log('RECEIVED: ', message);

  // modify the message
  message.logged = true;

  // continue processing the message
  next();

});

// Log every message sent
controller.middleware.send.use(function(bot, message, next) {

  // log it
  console.log('SENT: ', message);

  // modify the message
  message.logged = true;

  // continue processing the message
  next();

});
```
