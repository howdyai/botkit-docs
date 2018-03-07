# Botkit Bot Instances


#### bot.say()
| Argument | Description
|--- |---
| message | A message object
| callback | _Optional_ Callback in the form function(err,response) { ... }

Slack-specific Example:
```javascript
bot.say(
  {
    text: 'my message text',
    channel: 'C0H338YH4' // a valid slack channel, group, mpim, or im ID
  }
);
```
Note: If your primary need is to spontaneously send messages rather than respond to incoming messages, you may want to use [Slack's incoming webhooks feature](readme-slack.md#incoming-webhooks) rather than the real time API.


Facebook-specific Example:
```javascript
bot.say(
    {
        text: 'my message_text',
        channel: '+1(###)###-####' // a valid facebook user id or phone number
    }
);
```

#### bot.reply()
| Argument | Description
|--- |---
| message | Incoming message object
| reply | _String_ or _Object_ Outgoing response
| callback | _Optional_ Callback in the form function(err,response) { ... }

Simple reply example:
```javascript
controller.hears(['keyword','^pattern$'],['message_received'],function(bot,message) {

  // do something to respond to message
  // ...

  bot.reply(message,"Tell me more!");

});
```



#### bot.startConversation()
| Argument | Description
|---  |---
| message   | incoming message to which the conversation is in response
| callback  | a callback function in the form of  function(err,conversation) { ... }

`startConversation()` is a function that creates conversation in response to an incoming message.
The conversation will occur _in the same channel_ in which the incoming message was received.
Only the user who sent the original incoming message will be able to respond to messages in the conversation.

#### bot.startPrivateConversation()
| Argument | Description
|---  |---
| message   | message object containing {user: userId} of the user you would like to start a conversation with
| callback  | a callback function in the form of  function(err,conversation) { ... }

`startPrivateConversation()` is a function that initiates a conversation with a specific user. Note function is currently *Slack-only!*

#### bot.createConversation()
| Argument | Description
|---  |---
| message   | incoming message to which the conversation is in response
| callback  | a callback function in the form of  function(err,conversation) { ... }

This works just like `startConversation()`, with one main difference - the conversation
object passed into the callback will be in a dormant state. No messages will be sent,
and the conversation will not collect responses until it is activated using [convo.activate()](#conversationactivate).

Use `createConversation()` instead of `startConversation()` when you plan on creating more complex conversation structures using [threads](#conversation-threads) or [variables and templates](#using-variable-tokens-and-templates-in-conversation-threads) in your messages.

#### bot.createPrivateConversation()
| Argument | Description
|---  |---
| message   | incoming message to which the conversation is in response
| callback  | a callback function in the form of  function(err,conversation) { ... }

This works just like `startPrivateConversation()`, with one main difference - the conversation
object passed into the callback will be in a dormant state. No messages will be sent,
and the conversation will not collect responses until it is activated using [convo.activate()](#conversationactivate).
