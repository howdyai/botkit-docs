# Botkit Controllers



## Methods


### hears(keywords, events, middleware_or_cb, cb)

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


### on(event, cb, is_hearing)
NEEDS EXAMPLE

| Argument | Description
|--- |---
| x | x
| x  | x
| x | x
| x | x


https://github.com/howdyai/botkit/blob/dc0e780d3a50ffbfe89bc8f3908d1f8869d61466/lib/CoreBot.js#L1263

### trigger(event, data)
NEEDS EXAMPLE

| Argument | Description
|--- |---
| x | x
| x  | x
| x | x
| x | x


https://github.com/howdyai/botkit/blob/dc0e780d3a50ffbfe89bc8f3908d1f8869d61466/lib/CoreBot.js#L424

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
