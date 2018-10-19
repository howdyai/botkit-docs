# Botkit and Google Hangouts

Table of Contents

* [Getting Started](#getting-started)
* [Things to note](#things-to-note)
* [Create a Controller](#create-a-controller)
* [Event List](#event-list)

## Getting Started

1. [Install Botkit on your computer](/getstarted.html)

2. Create a Botkit powered Node app:

  * [Remix the starter project on Glitch](https://glitch.com/~botkit-hangouts)
  * Or: Use the command line tool:

  ```
  botkit new --platform googlehangouts
  ```

## Things to note

Hangouts Chat supports team collaboration by providing:

- Chat rooms that let you dedicate discussion space on a per-project, per-team, or other basis as appropriate
- Threaded conversations within rooms
- Direct messages between users
- Bots that can participate in rooms or respond to direct messages

Hangouts Chat communicates with your bot via an HTTP endpoint, you must specify an endpoint in the Chat API configuration tab when you publish your bot.
                                                               

## Create a Controller

To connect Botkit to Google Hangouts, use the constructor method, [Botkit.googlehangoutsbot()](#googlehangoutsbot).
This will create a Botkit controller with [all core features](core.md#botkit-controller-object) as well as [some additional methods](#additional-controller-methods).

#### Botkit.googlehangoutsbot()
| Argument | Description
|--- |---
| config | an object containing configuration options

Returns a new Botkit controller.

The `config` argument is an object with these properties:

| Name | Type | Description
|--- |--- |---
| studio_token | String | An API token from [Botkit Studio](#readme-studio.md)
| debug | Boolean | Enable debug logging
| endpoint | string | Bot endpoint link
| token| string | to verify bot authenticity


For example:

```javascript
var Botkit = require('botkit');
var controller = Botkit.googlehangoutsbot({
    endpoint: 'YOUR_ENDPOINT',
    token: "YOUR_TOKEN"
});
```


## Event List

In addition to the [core events that Botkit fires](core.md#receiving-messages-and-events), this connector also fires some platform specific events.

### Incoming Events

| Event | Description
|--- |---
| message_received | The bot received a direct message from a user
| direct_message | This event indicates that your bot was added to a DM
| bot_room_join | This event indicates that your bot was added to a shared room
| bot_room_leave | This event indicates that your bot was removed from a shared room
| bot_dm_join | This event indicates that your bot was added to a 1:1
| bot_dm_leave | This event indicates that your bot was removed from a 1:1
 
Here's an example on how to handler incoming events :
 
```javascript
controller.hears('hello', 'message_received', function (bot, message) { ... });
controller.hears('hi', 'direct_message', function (bot, message) { ... });
controller.on('bot_room_leave', function (bot, message) { ... });
```
## Send simple text messages

These messages are displayed like any other chat message. They can include simple character formatting.

[Learn more about simple text messages](https://developers.google.com/hangouts/chat/reference/message-formats/basic)

#### bot.reply()

| Argument | Description
|---  |---
| src | source message as received from google
| reply | reply message (string or object)
| callback | optional callback

This function sends an immediate response that is visible to everyone in the channel:

```javascript
bot.reply(message, `Hello`);
bot.reply(message, {text : `Hi !`});
````

#### bot.replyWithThreadKey()

| Argument | Description
|---  |---
| src | source message as received from google
| reply | reply message (string or object)
| callback | optional callback

To send a message as a reply in an existing thread:

```javascript
bot.replyWithThreadKey(message, {
    threadKey : "YOUR_THREAD_KEY",
    requestBody : {
        text : `Hi ! this message inside the same thread`
    }
});
````

#### bot.replyAsNewThread()

| Argument | Description
|---  |---
| src | source message as received from google
| reply | reply message (string or object)
| callback | optional callback

To send a message into Hangouts Chat as a new thread:

```javascript
bot.replyAsNewThread(message, `Hello ! this is a new thread`);
````

## Send card messages

These messages contain all the details that Hangouts Chat needs to render a card in the chat room. These details include any text, special formatting, widgets, and other details used in the card.

[Learn more about card messages](https://developers.google.com/hangouts/chat/reference/message-formats/cards)


```javascript
controller.hears('cards', 'message_received', function (bot, message) {
    bot.reply(message, {
        requestBody: {
            cards: [
                {
                    "sections": [
                        {
                            "widgets": [
                                {
                                    "image": { "imageUrl": "https://image.slidesharecdn.com/botkitsignal-160526164159/95/build-a-bot-with-botkit-1-638.jpg?cb=1464280993" }
                                },
                                {
                                    "buttons": [
                                        {
                                            "textButton": {
                                                "text": "Get Started",
                                                "onClick": {
                                                    "openLink": {
                                                        "url": "https://botkit.ai/docs/"
                                                    }
                                                }
                                            }
                                        }
                                    ]
                                }
                            ]
                        }
                    ]
                }
            ]
        }
    });
});
```