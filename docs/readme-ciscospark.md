# Botkit and Cisco Webex Teams

*Note: Cisco Spark is [now Cisco Webex Teams](https://blogs.cisco.com/collaboration/webex-platform-convergence?_ga=2.75241682.1559193461.1524070554-1809816543.1524070554). Expects changes to this document as the transition continues, but for now you may use them interchangably*

Table of Contents

* [Getting Started](#getting-started)
* [Create a Controller](#create-a-controller)
* [Webex Teams-specific Events](#event-list)
* [Message Formatting](#message-formatting)
* [Attaching Files](#attaching-files)
* [Receiving Files](#receiving-files)
* [Starting Direct Messages](#starting-direct-messages)


## Getting Started

1. [Install Botkit on your computer](/getstarted.html)

2. Create a Botkit powered Node app:

  * [Deploy a pre-configured app using Botkit Studio](https://studio.botkit.ai)
  * Or: [Remix the starter project on Glitch](https://glitch.com/~botkit-ciscospark)
  * Or: Use the command line tool:

  ```
  botkit new --platform spark
  ```

3. [Follow this guide to configuring the Cisco Webex Teams API](/docs/provisioning/cisco-spark.md)



## Working with Cisco Webex Teams

Botkit receives messages from Cisco Webex Teams using webhooks, and sends messages using their APIs. This means that your bot application must present a web server that is publicly addressable. Everything you need to get started is already included in Botkit.

To connect your bot to Cisco Webex Teams, [get an access token here](https://developer.ciscospark.com/add-bot.html). In addition to the access token,
Cisco Webex Teams bots require a user-defined `secret` which is used to validate incoming webhooks, as well as a `public_address` which is the URL at which the bot application can be accessed via the internet.

Each time the bot application starts, Botkit will register a webhook subscription.
Botkit will automatically manage your bot's webhook subscriptions, but if you plan on having multiple instances of your bot application with different URLs (such as a development instance and a production instance), use the `webhook_name` field with a different value for each instance.

Bots in Cisco Webex Teams are identified by their email address, and can be added to any space in any team or organization. If your bot should only be available to users within a specific organization, use the `limit_to_org` or `limit_to_domain` options.
This will configure your bot to respond only to messages from members of the specific organization, or whose email addresses match one of the specified domains.

The full code for a simple Cisco Webex Teams bot is below:

```javascript
var Botkit = require('./lib/Botkit.js');

var controller = Botkit.sparkbot({
    debug: true,
    log: true,
    public_address: process.env.public_address,
    ciscospark_access_token: process.env.access_token,
    secret: process.env.secret
});


var bot = controller.spawn({
});

controller.setupWebserver(process.env.PORT || 3000, function(err, webserver) {
    controller.createWebhookEndpoints(webserver, bot, function() {
        console.log("SPARK: Webhooks set up!");
    });
});

controller.hears('hello', 'direct_message,direct_mention', function(bot, message) {
    bot.reply(message, 'Hi');
});

controller.on('direct_mention', function(bot, message) {
    bot.reply(message, 'You mentioned me and said, "' + message.text + '"');
});

controller.on('direct_message', function(bot, message) {
    bot.reply(message, 'I got your private message. You said, "' + message.text + '"');
});

```

## Create a Controller

To connect Botkit to Cisco Webex Teams, use the Spark constructor method, [Botkit.sparkbot()](#botkitsparkbot).
This will create a Botkit controller with [all core features](core.md#botkit-controller-object) as well as [some additional methods](#additional-controller-methods).

#### Botkit.sparkbot()
| Argument | Description
|--- |---
| studio_token | String | An API token from [Botkit Studio](#readme-studio.md)
| debug | Boolean | Enable debug logging
| public_address | _required_ the root url of your application (https://mybot.com)
| `ciscospark_access_token` | _required_ token provided by Cisco Webex Teams for your bot
| secret | _required_ secret for validating webhooks originate from Cisco Webex Teams
| webhook_name | _optional_ name for webhook configuration on Cisco Webex Teams side. Providing a name here allows for multiple bot instances to receive the same messages. Defaults to 'Botkit Firehose'
| `limit_to_org` | _optional_ organization id in which the bot should exist. If user from outside org sends message, it is ignored
| `limit_to_domain` | _optional_ email domain (@howdy.ai) or array of domains [@howdy.ai, @botkit.ai] from which messages can be received

```javascript
var controller = Botkit.sparkbot({
    debug: true,
    log: true,
    public_address: 'https://mybot.ngrok.io',
    ciscospark_access_token: process.env.access_token,
    secret: 'randomstringofnumbersandcharacters12345',
    webhook_name: 'dev',
    limit_to_org: 'my_spark_org_id',
    limit_to_domain: ['@howdy.ai','@cisco.com'],
});
```

## Event List

In addition to the [core events that Botkit fires](core.md#receiving-messages-and-events), this connector also fires some  platform specific events.

All events [listed here](https://developer.ciscospark.com/webhooks-explained.html#resources-events) should be expected, in the format `resource`.`event` - for example, `rooms.created`.  

### Incoming Message Events
| Event | Description
|--- |---
| direct_message | Bot has received a message as a DM
| direct_mention | Bot has been mentioned in a public space
| self_message | Bot has received a message it sent

### User Activity Events:
| Event | Description
|--- |---
| user_space_join | a user has joined a space in which the bot is present
| bot_space_join | the bot has joined a new space
| user_space_leave | a user has left a space in which the bot is present
| bot_space_leave | the bot has left a space


## Message Formatting

Cisco Webex Teams supports both a `text` field and a `markdown` field for outbound messages. [Read here for details on Cisco Webex Teams's markdown support.](https://developer.ciscospark.com/formatting-messages.html)

To specify a markdown version, add it to your message object:

```javascript
bot.reply(message,{text: 'Hello', markdown: '*Hello!*'});
```

## Attaching Files

Files can be attached to outgoing messages in one of two ways.

*Specify URL*

If the file you wish to attach is already available online, simply specify the URL in the `files` field of the outgoing message:

```javascript
bot.reply(message,{text:'Here is your file!', files:['http://myserver.com/file.pdf']});
```

*Send Local File*

If the file you wish to attach is present only on the local server, attach it to the message as a readable stream:

```javascript
var fs = require('fs');
bot.reply(message,{text: 'I made this file for you.', files:[fs.createReadStream('./newfile.txt')]});
```

## Receiving files

Your bot may receive messages with files attached. Attached files will appear in an array called `message.original_message.files`.

Botkit provides 2 methods for retrieving information about the file.

### bot.retrieveFileInfo(url, cb)
| Parameter | Description
|--- |---
| url | url of file from message.original_message.files
| cb | callback function in the form function(err, file_info)

The callback function will receive an object with fields like `filename`, `content-type`, and `content-length`.

```javascript
controller.on('direct_message', function(bot, message) {
    bot.reply(message, 'I got your private message. You said, "' + message.text + '"');
    if (message.original_message.files) {
        bot.retrieveFileInfo(message.original_message.files[0], function(err, file_info) {
            bot.reply(message,'I also got an attached file called ' + file_info.filename);
        });
    }
});
```

### bot.retrieveFile(url, cb)
| Parameter | Description
|--- |---
| url | url of file from message.original_message.files
| cb | callback function in the form function(err, file_content)

The callback function will receive the full content of the file.

```javascript
controller.on('direct_message', function(bot, message) {
    bot.reply(message, 'I got your private message. You said, "' + message.text + '"');
    if (message.original_message.files) {
        bot.retrieveFileInfo(message.original_message.files[0], function(err, file_info) {
            if (file_info['content-type'] == 'text/plain') {
                bot.retrieveFile(message.original_message.files[0], function(err, file) {
                    bot.reply(message,'I got a text file with the following content: ' + file);
                });
            }
        });
    }
});
```

## Starting Direct Messages

Cisco Webex Teams's API provides several ways to send private messages to users -
by the user's email address, or by their user id. These may be used in the case where the
user's email address is unknown or unavailable, or when the bot should respond to the `actor`
instead of the `sender` of a message.

For example, a bot may use these methods when handling a `bot_space_join` event
in order to send a message to the _user who invited the bot_ (the actor) instead of
the bot itself (the sender).

NOTE: Core functions like [bot.startPrivateConversation()](core.md#botstartprivateconversation) work as expected,
and will create a direct message thread with the sender of the incoming_message.

### bot.startPrivateConversationWithPersonId()
| Parameter | Description
|--- |---
| personId | the personId of the user to whom the bot should send a message
| cb | callback function in the form function(err, file_content)

Use this function to send a direct message to a user by their personId, which
can be found in message and event payloads at the following location:

```javascript
var personId = message.original_message.actorId;
```

### bot.startPrivateConversationWithActor())
| Parameter | Description
|--- |---
| incoming_message | a message or event that has an actorId defined in message.original_message.actorId
| cb | callback function in the form function(err, file_content)

```javascript
controller.on('bot_space_join', function(bot, message) {
  bot.startPrivateConversationWithActor(message, function(err, convo) {
    convo.say('The bot you invited has joined the channel.');
  });
});
```


# Additional Controller methods

#### controller.createWebhookEndpoints()

#### controller.resetWebhookSubscriptions()

#### controller.handleWebhookPayload()
