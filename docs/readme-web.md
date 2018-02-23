# Botkit Web Connector

The power of bots and conversational software can included in any website or app using Botkit's new connector!

## What is this?

This connector provides a basic interface for Botkit to send and receive messages directly, rather than relying on a third party messaging service like Slack or Facebook messenger.
It includes a built-in chat server that can send and receive messages using real-time websocket connections or asynchronous webhooks.
This allows Botkit bots to live on the web, or be embedded into websites and native mobile apps.

The functionality provided in the connector is actually very simple, and requires integration with a webserver (to serve the application and host the chat server) and a front-end client (to provide a user interface and render messages). These components are provided in a separate project, [Botkit Anywhere](https://github.com/howdyai/botkit-starter-web).

## Getting Started

Everything you need to build your bot is included in [Botkit Anywhere](https://github.com/howdyai/botkit-starter-web),
a boilerplate application that includes all the components needed to operate your bot, as well as sample code and extra features.

* All the features of Botkit Core and Botkit Studio
* A built-in chat server that can handle thousands of simultaneous conversations
* A customizable front-end chat client built with HTML, CSS, and Javascript
* A webserver for serving the application to users and hosting your bot's business logic

**Most developers should start with [the starter kit](https://github.com/howdyai/botkit-starter-web) rather than make direct use of the Botkit core library.**

## Developing with Botkit for the Web

### Events

When using [Botkit Anywhere](https://github.com/howdyai/botkit-starter-web) and the built-in [web chat client](https://github.com/howdyai/botkit-starter-web/blob/master/docs/botkit_web_client.md), Botkit will fire a small number of native events.

#### Built-in Events

| Event | Description
|-- |--
| message_received | the bot has received a message
| hello | a new user has connected to the bot
| welcome_back | a returning user has established a new connection to the bot
| reconnect | an ongoing user session has experienced a disconnect/reconnect

#### Custom Events

Developers may also fire custom events from the web client. Custom events triggered in the front-end client will arrive at the Botkit app based on their `type` field.

For example, [the remoteTriggers skill included in Botkit Anywhere](https://github.com/howdyai/botkit-starter-web/blob/master/skills/remoteTriggers.js) uses a custom event type to request that the bot load and execute a script from Botkit Studio's content management API.

Custom events like this can be used to build new methods for your user to interact with the bot, such as buttons or other interactive elements.

### Message Objects

The message objects used by Botkit Web are essentially "pure" Botkit messages, containing a few primary fields:

| Field | Description
|-- |--
| user | a unique user id for the user
| text | the text of the message
| type | type of message or event

Botkit has a very liberal policy on adding additional fields to the message object, so depending on the implementation details of the front-end client, other fields may be present!

In addition, the Botkit Studio and the built-in web chat client feature support for quick replies and file attachments to be included in messages.

#### Quick Replies

Quick replies are buttons that appear at the bottom of the message client, and offer suggested replies to the user.  Clicking a quick reply is essentially the same as the user typing the suggested reply and sending it as a message.

To add quick replies to Botkit Web messages, include a `quick_replies` field that includes an array of objects, each with a `title` and a `payload`. The title will displayed on the button itself, while the payload is the actual text sent back to the bot by the user.

```js
var reply = {
  text: 'Look, quick replies!',
  quick_replies: [
      {
          title: 'Hello',
          payload: 'hello'
      },
      {
          title: 'Help',
          payload: 'help'
      },
  ]
}
```


#### File Attachments

Files can be attached to Botkit Web messages by adding a `files` field that includes an array of objects, each with a `url` and an `image` field. The url field should contain a valid URL, and the image should be set to `true` if the file is an image.

```js
var reply = {
  text: 'Look, an image!',
  files: [
      {
        url: 'http://tableflipper.com/IRX2.gif',
        image: true
      }
  ]
}
```

## Functions Index

Botkit on the web works much the same as it does on all the other platforms. Developers have full access to all of [Botkit's core features](core.md) -
and a few extra ones that are specific to operating on the web!

### controller.openSocketServer(webserver_instance)

This function takes an instance of an http server object - that is, the results of a call to `http.createServer()`.

It connects a `ws` powered websocket server to the web server, and allows the application to accept socket connections directly with the bot.

[A compatible webserver is provided in the starter kit](https://github.com/howdyai/botkit-starter-web/blob/master/components/express_webserver.js).


### bot.replyWithTyping(message, reply)

This works just like the normal `bot.reply()`, but instead of sending the message immediately, sends a typing indicator first, then waits for a short period before sending the actual message.
