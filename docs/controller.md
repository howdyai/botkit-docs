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

https://github.com/howdyai/botkit/blob/dc0e780d3a50ffbfe89bc8f3908d1f8869d61466/lib/CoreBot.js#L1263

### trigger(event, data)
NEEDS EXAMPLE
https://github.com/howdyai/botkit/blob/dc0e780d3a50ffbfe89bc8f3908d1f8869d61466/lib/CoreBot.js#L424

### hears_regexp(tests, message)
NEEDS EXAMPLE
hears_regexp - default string matcher uses regular expressions

found in:
https://github.com/howdyai/botkit/blob/dc0e780d3a50ffbfe89bc8f3908d1f8869d61466/lib/CoreBot.js#L1173
https://github.com/howdyai/botkit/blob/dc0e780d3a50ffbfe89bc8f3908d1f8869d61466/lib/Botkit.d.ts#L96


### changeEars(new_test)
Needs Example

https://github.com/howdyai/botkit/blob/dc0e780d3a50ffbfe89bc8f3908d1f8869d61466/lib/CoreBot.js#L1211
https://github.com/howdyai/botkit/blob/dc0e780d3a50ffbfe89bc8f3908d1f8869d61466/lib/Botkit.d.ts#L120

### excludeFromConversations(event)
Needs example

https://github.com/howdyai/botkit/blob/e196bfa1b89657d36662dffe81f3b9672fb257a2/docs/readme.md#excluding-events-from-conversations

For example:
```javascript
// always exclude facebook postback events from conversations
controller.excludeFromConversations('facebook_postback')
```
### spawn(config, cb)
Needs example, but obviously there is a version of this in each integration.js

### defineBot(unit)
needs example

https://github.com/howdyai/botkit/blob/dc0e780d3a50ffbfe89bc8f3908d1f8869d61466/lib/CoreBot.js#L1335

### setTickDelay(delay)
### startTicking()

### setupWebserver(port, cb)

### userAgent()
### version()
### shutdown()




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
