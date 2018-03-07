# Botkit Controller Object 

#### controller.hears()
| Argument | Description
|--- |---
| patterns | An _array_ or a _comma separated string_ containing a list of regular expressions to match
| types  | An _array_ or a _comma separated string_ of the message events in which to look for the patterns
| middleware function | _optional_ function to redefine how patterns are matched. see [Botkit Middleware](middleware.md)
| callback | callback function that receives a message object

Example:
```javascript
controller.hears(['keyword','^pattern$'],['message_received'],function(bot,message) {

  // do something to respond to message
  bot.reply(message,'You used a keyword!');

});
```

When using the built in regular expression matching, the results of the expression will be stored in the `message.match` field and will match the expected output of normal Javascript `string.match(/pattern/i)`. 

For example:

```javascript
controller.hears('open the (.*) doors',['message_received'],function(bot,message) {
  var doorType = message.match[1]; //match[1] is the (.*) group. match[0] is the entire group (open the (.*) doors).
  if (doorType === 'pod bay') {
    return bot.reply(message, 'I\'m sorry, Dave. I\'m afraid I can\'t do that.');
  }
  return bot.reply(message, 'Okay');
});
```


### controller.on()
| Argument | Description
|--- |---
| event_name | a string or array containing an event or comma-separated list of events
| callback  | callback function in the form of form of function(bot, event) {...}

Handle events emitted by Botkit. The vast majority of events will call a callback function with 2 parameters - a bot instance, and the event object.  

[Read about receiving and handling events](core.md#responding-to-events)

```javascript
// handle a message event
controller.on('message_received', function(bot, message) {
  bot.reply(message,'Received');
});

// handle a channel join event
controller.on('channel_join', function(bot, event) {
  bot.reply(event,'Welcome to the channel!');
});
```

Note that you may also trigger your own events using [controller.trigger()](#controllertrigger) and handle them. This can be 
useful for separating the trigger logic from the actual event handlers, as in the example below.

```javascript

// listen for a help request.. and then trigger a help_request event.
controller.hears('help', 'message_received', function(bot, message) {
  // this event can be triggered whenever a user needs help
  bot.trigger('help_request', [bot, message]);
});

controller.on('help_request', function(bot, message) {
  
  bot.reply(message,'I am here to help!');

});
```

#### controller.trigger()
| Argument | Description
|--- |---
| event_name | the name of a custom event
| parameters | an array of parameters to be passed on to any handler functions

Triggers a custom event, which can then be handled by [controller.on()](#controlleron).

The second argument to `controller.trigger()` is an array which should contain an array of parameters that will be passed on to any handler functions. While not required, it is highly recommended that custom events conform to the pattern used by native Botkit events and include a bot instance as the first parameter, and if applicable, a normalized Botkit event object as the second paramter:

```javascript
controller.trigger('my_custom_event', [bot, event]);
```

### hears_regexp(tests, message)
NEEDS EXAMPLE

| Argument | Description
|--- |---
| x | x
| x  | x
| x | x
| x | x


hears_regexp - default string matcher uses regular expressions

found in:
https://github.com/howdyai/botkit/blob/dc0e780d3a50ffbfe89bc8f3908d1f8869d61466/lib/CoreBot.js#L1173
https://github.com/howdyai/botkit/blob/dc0e780d3a50ffbfe89bc8f3908d1f8869d61466/lib/Botkit.d.ts#L96


### changeEars(new_test)
Needs Example

| Argument | Description
|--- |---
| x | x
| x  | x
| x | x
| x | x


https://github.com/howdyai/botkit/blob/dc0e780d3a50ffbfe89bc8f3908d1f8869d61466/lib/CoreBot.js#L1211
https://github.com/howdyai/botkit/blob/dc0e780d3a50ffbfe89bc8f3908d1f8869d61466/lib/Botkit.d.ts#L120

### excludeFromConversations(event)
Needs example

| Argument | Description
|--- |---
| x | x
| x  | x
| x | x
| x | x


https://github.com/howdyai/botkit/blob/e196bfa1b89657d36662dffe81f3b9672fb257a2/docs/readme.md#excluding-events-from-conversations

For example:
```javascript
// always exclude facebook postback events from conversations
controller.excludeFromConversations('facebook_postback')
```
### spawn(config, cb)
Needs example, but obviously there is a version of this in each integration.js

| Argument | Description
|--- |---
| x | x
| x  | x
| x | x
| x | x


### defineBot(unit)
needs example

| Argument | Description
|--- |---
| x | x
| x  | x
| x | x
| x | x


https://github.com/howdyai/botkit/blob/dc0e780d3a50ffbfe89bc8f3908d1f8869d61466/lib/CoreBot.js#L1335

### setTickDelay(delay)

| Argument | Description
|--- |---
| x | x
| x  | x
| x | x
| x | x


https://github.com/howdyai/botkit/blob/e196bfa1b89657d36662dffe81f3b9672fb257a2/docs/readme.md#changing-the-speed-of-botkits-internal-tick

For example:
``` javascript
// only wait 100ms between conversation loops
controller.setTickDelay(100);
```

### startTicking()

Needs example, but there is a version of this in each integration.js

| Argument | Description
|--- |---
| x | x
| x  | x
| x | x
| x | x

Why is this handled differently per integration?

### setupWebserver(port, cb)

| Argument | Description
|--- |---
| x | x
| x  | x
| x | x
| x | x

https://github.com/howdyai/botkit/blob/dc0e780d3a50ffbfe89bc8f3908d1f8869d61466/lib/Botkit.d.ts#L124 (appears in integrations as well)

### userAgent()

Need examples

| Argument | Description
|--- |---
| x | x
| x  | x
| x | x
| x | x

https://github.com/howdyai/botkit/blob/dc0e780d3a50ffbfe89bc8f3908d1f8869d61466/lib/CoreBot.js#L1525

### version()
needs examples

| Argument | Description
|--- |---
| x | x
| x  | x
| x | x
| x | x

Couldnt find this!

### shutdown()

| Argument | Description
|--- |---
| x | x
| x  | x
| x | x
| x | x

needs example!
https://github.com/howdyai/botkit/blob/dc0e780d3a50ffbfe89bc8f3908d1f8869d61466/lib/CoreBot.js#L1410


## Middlewares
needs explaination, the only reference to these is in code:
https://github.com/howdyai/botkit/blob/dc0e780d3a50ffbfe89bc8f3908d1f8869d61466/lib/CoreBot.js#L50

    spawn: ware(),
    ingest: ware(),
    normalize: ware(),
    categorize: ware(),
    receive: ware(),
    heard: ware(), // best place for heavy i/o because fewer messages
    triggered: ware(), // like heard, but for other events
    capture: ware(),
    format: ware(),
    send: ware(),
    conversationStart: ware(),
    conversationEnd: ware(),
