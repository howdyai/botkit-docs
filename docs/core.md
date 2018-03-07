# Botkit Core

Bots built with Botkit have a few key capabilities, which can be used to create clever, conversational applications. These capabilities map to the way real human people talk to each other.

Bots can [hear things](#receiving-messages), [say things and reply](#sending-messages) to what they hear.

With these two building blocks, almost any type of conversation can be created.

To organize the things a bot says and does into useful units, Botkit bots have a subsystem available for managing [multi-message conversations](#multi-message-conversations). Conversations add features like the ability to ask a question, queue several messages at once, and track when an interaction has ended.  Handy!

## Developing with Botkit

Table of Contents

* [Setting up a Botkit Controller](#botkit-controller)
  * [Botkit Controller Methods List](#botkit-controller-object)
* [Receiving Messages](#receiving-messages)
* [Sending Messages](#sending-messages)
* [Multi-message Conversations](#multi-message-conversations)
* [Middleware](middleware.md)
* [Advanced Topics](#advanced-topics)


## Setting up a Botkit Controller

The robot brain inside every Botkit applications is the `controller`, an interface that is used to define all the features and functionality of an app. Botkit's core library provides a platform-independent interface for sending and receiving messages so that bots on any platform can be created using the same set of tools.

By attaching event handlers to the controller object, developers can specify what type of messages and events their bot should look for and respond to, including keywords, patterns and status events. These event handlers can be thought of metaphorically as skills or features the robot brain has -- each event handler defines a new "When a human says THIS the bot does THAT."

Once created, the controller will handle incoming messages, [spawn bot instances](#controller-spawn) and [trigger handlers](#responding-to-events).

For each platform, there is a specialized version of the controller object. These specialized controllers customize Botkit's core features to work with the platform, and add additional features above and beyond core that offer developers access platform-specific features.

Each platform has its own set of configuration options - refer to the platform connector docs for details:

* [Web and Apps](readme-web.md#create-a-controller)
* [Slack](readme-slack.md#create-a-controller)
* [Cisco Spark](readme-ciscospark.md#create-a-controller)
* [Microsoft Teams](readme-teams.md#create-a-controller)
* [Facebook Messenger](readme-facebook.md#create-a-controller)
* [Twilio SMS](readme-twiliosms.md#create-a-controller)
* [Twilio IPM](readme-twilioipm.md#create-a-controller)
* [Microsoft Bot Framework](readme-botframework.md#create-a-controller)

```javascript
var Botkit = require('botkit');

var controller = Botkit.anywhere(configuration);

// give the bot something to listen for.
controller.hears('hello','message_received',function(bot,message) {
  bot.reply(message,'Hello yourself.');

});
```

## Responding to events

Once connected to a messaging platform, bots receive a constant stream of events - everything from the normal messages you would expect to typing notifications and presence change events. The set of events your bot will receive will depend on what messaging platform it is connected to.

All platforms will receive the `message_received` event. This event is the first event fired for every message of any type received - before any platform specific events are fired.

```javascript
controller.on('message_received', function(bot, message) {

    // carefully examine and
    // handle the message here!
    // Note: Platforms such as Slack send many kinds of messages, not all of which contain a text field!
});
```

Due to the multi-channel, multi-user nature of Slack, Botkit does additional filtering on the messages (after firing message_received), and will fire more specific events based on the type of message - for example, `direct_message` events indicate a message has been sent directly to the bot, while `direct_mention` indicates that the bot has been mentioned in a multi-user channel.
[List of Slack-specific Events](readme-slack.md#slack-specific-events)

Similarly, bots in Cisco Spark will receive `direct_message` events to indicate a message has been sent directly to the bot, while `direct_mention` indicates that the bot has been mentioned in a multi-user channel. Several other Spark-specific events will also fire. [List of Cisco Spark-specific Events](readme-ciscospark.md#spark-specific-events)

Twilio IPM bots can also exist in a multi-channel, multi-user environment. As a result, there are many additional events that will fire. In addition, Botkit will filter some messages, so that the bot will not receive it's own messages or messages outside of the channels in which it is present.
[List of Twilio IPM-specific Events](readme-twilioipm.md#twilio-ipm-specific-events)

Facebook messages are fairly straightforward. However, because Facebook supports inline buttons, there is an additional event fired when a user clicks a button.
[List of Facebook-specific Events](readme-facebook.md#facebook-specific-events)


## Receiving Messages

Botkit bots receive messages through a system of specialized event handlers. Handlers can be set up to respond to specific types of messages, or to messages that match a given keyword or pattern.

These message events can be handled by attaching an event handler to the main controller object.
These event handlers take two parameters: the name of the event, and a callback function which is invoked whenever the event occurs.
The callback function receives a bot object, which can be used to respond to the message, and a message object.

```javascript
// reply to any incoming message
controller.on('message_received', function(bot, message) {
    bot.reply(message, 'I heard... something!');
});

// reply to a direct mention - @bot hello
controller.on('direct_mention',function(bot,message) {
  // reply to _message_ by using the _bot_ object
  bot.reply(message,'I heard you mention me!');
});

// reply to a direct message
controller.on('direct_message',function(bot,message) {
  // reply to _message_ by using the _bot_ object
  bot.reply(message,'You are talking directly to me');
});
```

### Matching Patterns and Keywords with `hears()`

In addition to these traditional event handlers, Botkit also provides the [controller.hears()](#controller-hears) function,
which configures event handlers based on matching specific keywords or phrases in the message text.
The hears function works just like the other event handlers, but takes a third parameter which
specifies the keywords to match.

```javascript
controller.hears(['keyword','^pattern$'],['message_received'],function(bot,message) {

  // do something to respond to message
  bot.reply(message,'You used a keyword!');

});
```

When using the built in regular expression matching, the results of the expression will be stored in the `message.match` field and will match the expected output of normal Javascript `string.match(/pattern/i)`. For example:

```javascript
controller.hears('open the (.*) doors',['message_received'],function(bot,message) {
  var doorType = message.match[1]; //match[1] is the (.*) group. match[0] is the entire group (open the (.*) doors).
  if (doorType === 'pod bay') {
    return bot.reply(message, 'I\'m sorry, Dave. I\'m afraid I can\'t do that.');
  }
  return bot.reply(message, 'Okay');
});
```

## Sending Messages

Bots have to send messages to deliver information and present an interface for their
functionality.  Botkit bots can send messages in several different ways, depending
on the type and number of messages that will be sent.

Single message replies to incoming commands can be sent using the `bot.reply()` function.

Multi-message replies, particularly those that present questions for the end user to respond to,
can be sent using the `bot.startConversation()` function and the related conversation sub-functions.

Bots can originate messages - that is, send a message based on some internal logic or external stimulus -
using `bot.say()` method.

All `message` objects must contain a `text` property, even if it's only an empty string.

### Single Message Replies to Incoming Messages

Once a bot has received a message using a `on()` or `hears()` event handler, a response
can be sent using `bot.reply()`.

Messages sent using `bot.reply()` are sent immediately. If multiple messages are sent via
`bot.reply()` in a single event handler, they will arrive in the  client very quickly
and may be difficult for the user to process. We recommend using `bot.startConversation()`
if more than one message needs to be sent.

You may pass either a string, or a message object to the function.

Message objects may also contain any additional fields supported by the messaging platform in use:

[Slack's chat.postMessage](https://api.slack.com/methods/chat.postMessage) API accepts several additional fields. These fields can be used to adjust the message appearance, add attachments, or even change the displayed user name.

This is also true of Facebook. Calls to [Facebook's Send API](https://developers.facebook.com/docs/messenger-platform/send-api-reference) can include attachments which result in interactive "structured messages" which can include images, links and action buttons.

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

Slack-specific fields and attachments:
```javascript
controller.on('ambient',function(bot,message) {

    // do something...

    // then respond with a message object
    //
    bot.reply(message,{
      text: "A more complex response",
      username: "ReplyBot",
      icon_emoji: ":dash:",
    });

})

//Using attachments
controller.hears('another_keyword','direct_message,direct_mention',function(bot,message) {
  var reply_with_attachments = {
    'username': 'My bot' ,
    'text': 'This is a pre-text',
    'attachments': [
      {
        'fallback': 'To be useful, I need you to invite me in a channel.',
        'title': 'How can I help you?',
        'text': 'To be useful, I need you to invite me in a channel ',
        'color': '#7CD197'
      }
    ],
    'icon_url': 'http://lorempixel.com/48/48'
    }

  bot.reply(message, reply_with_attachments);
});

```


Facebook-specific fields and attachments:
```javascript
// listen for the phrase `shirt` and reply back with structured messages
// containing images, links and action buttons
controller.hears(['shirt'],'message_received',function(bot, message) {
    bot.reply(message, {
        attachment: {
            'type':'template',
            'payload':{
                 'template_type':'generic',
                 'elements':[
                   {
                     'title':'Classic White T-Shirt',
                     'image_url':'http://petersapparel.parseapp.com/img/item100-thumb.png',
                     'subtitle':'Soft white cotton t-shirt is back in style',
                     'buttons':[
                       {
                         'type':'web_url',
                         'url':'https://petersapparel.parseapp.com/view_item?item_id=100',
                         'title':'View Item'
                       },
                       {
                         'type':'web_url',
                         'url':'https://petersapparel.parseapp.com/buy_item?item_id=100',
                         'title':'Buy Item'
                       },
                       {
                         'type':'postback',
                         'title':'Bookmark Item',
                         'payload':'USER_DEFINED_PAYLOAD_FOR_ITEM100'
                       }
                     ]
                   },
                   {
                     'title':'Classic Grey T-Shirt',
                     'image_url':'http://petersapparel.parseapp.com/img/item101-thumb.png',
                     'subtitle':'Soft gray cotton t-shirt is back in style',
                     'buttons':[
                       {
                         'type':'web_url',
                         'url':'https://petersapparel.parseapp.com/view_item?item_id=101',
                         'title':'View Item'
                       },
                       {
                         'type':'web_url',
                         'url':'https://petersapparel.parseapp.com/buy_item?item_id=101',
                         'title':'Buy Item'
                       },
                       {
                         'type':'postback',
                         'title':'Bookmark Item',
                         'payload':'USER_DEFINED_PAYLOAD_FOR_ITEM101'
                       }
                     ]
                   }
                 ]
               }
        }
    });
});
```

## Multi-message Conversations

For more complex commands, multiple messages may be necessary to send a response,
particularly if the bot needs to collect additional information from the user.

Botkit provides a `Conversation` object type that is used to string together several
messages, including questions for the user, into a cohesive unit. Botkit conversations
provide useful methods that enable developers to craft complex conversational
user interfaces that may span a several minutes of dialog with a user, without having to manage
the complexity of connecting multiple incoming and outgoing messages across
multiple API calls into a single function.

Messages sent as part of a conversation are sent no faster than one message per second,
which roughly simulates the time it would take for the bot to "type" the message.


### Conversation Threads

While conversations with only a few questions can be managed by writing callback functions,
more complex conversations that require branching, repeating or looping sections of dialog,
or data validation can be handled using feature of the conversations we call `threads`.

Threads are pre-built chains of dialog between the bot and end user that are built before the conversation begins. Once threads are built, Botkit can be instructed to navigate through the threads automatically, allowing many common programming scenarios such as yes/no/quit prompts to be handled without additional code.

You can build conversation threads in code, or you can use [Botkit Studio](readme-studio.md)'s script management tool to build them in a friendly web environment. Conversations you build yourself and conversations managed in Botkit Studio work the same way -- they run inside your bot and use your code to manage the outcome.

If you've used the conversation system at all, you've used threads - you just didn't know it. When calling `convo.say()` and `convo.ask()`, you were actually adding messages to the `default` conversation thread that is activated when the conversation object is created.


### Start a Conversation

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

### Control Conversation Flow


### Automatically Switch Threads using Actions

You can direct a conversation to switch from one thread to another automatically
by including the `action` field on a message object. Botkit will switch threads immediately after sending the message.

```javascript
// first, define a thread called `next_step` that we'll route to...
convo.addMessage({
    text: 'This is the next step...',
},'next_step');


// send a message, and tell botkit to immediately go to the next_step thread
convo.addMessage({
    text: 'Anyways, moving on...',
    action: 'next_step'
});
```

Developers can create fairly complex conversational systems by combining these message actions with conditionals in `ask()` and `addQuestion()`.  Actions can be used to specify
default or next step actions, while conditionals can be used to route between threads.

From inside a callback function, use `convo.gotoThread()` to instantly switch to a different pre-defined part of the conversation. Botkit can be set to automatically navigate between threads based on user input, such as in the example below.

```javascript
bot.createConversation(message, function(err, convo) {

    // create a path for when a user says YES
    convo.addMessage({
            text: 'You said yes! How wonderful.',
    },'yes_thread');

    // create a path for when a user says NO
    convo.addMessage({
        text: 'You said no, that is too bad.',
    },'no_thread');

    // create a path where neither option was matched
    // this message has an action field, which directs botkit to go back to the `default` thread after sending this message.
    convo.addMessage({
        text: 'Sorry I did not understand.',
        action: 'default',
    },'bad_response');

    // Create a yes/no question in the default thread...
    convo.addQuestion('Do you like cheese?', [
        {
            pattern: 'yes',
            callback: function(response, convo) {
                convo.gotoThread('yes_thread');
            },
        },
        {
            pattern: 'no',
            callback: function(response, convo) {
                convo.gotoThread('no_thread');
            },
        },
        {
            default: true,
            callback: function(response, convo) {
                convo.gotoThread('bad_response');
            },
        }
    ],{},'default');

    convo.activate();
});
```

### Special Actions

In addition to routing from one thread to another using actions, you can also use
one of a few reserved words to control the conversation flow.

Set the action field of a message to `completed` to end the conversation immediately and mark as success.

Set the action field of a message to `stop` end immediately, but mark as failed.

Set the action field of a message to `timeout` to end immediately and indicate that the conversation has timed out.

After the conversation ends, these values will be available in the `convo.status` field. This field can then be used to check the final outcome of a conversation. See [handling the end of conversations](#handling-end-of-conversation).

### Using Variable Tokens and Templates in Conversation Threads

Pre-defined conversation threads are great, but many times developers will need to inject dynamic content into a conversation.
Botkit achieves this by processing the text of every message using the [Mustache template language](https://mustache.github.io/).
Mustache offers token replacement, as well as access to basic iterators and conditionals.

Variables can be added to a conversation at any point after the conversation object has been created using the function `convo.setVar()`. See the example below.

```javascript
convo.createConversation(message, function(err, convo) {

    // .. define threads which will use variables...
    // .. and then, set variable values:
    convo.setVar('foo','bar');
    convo.setVar('list',[{value:'option 1'},{value:'option 2'}]);
    convo.setVar('object',{'name': 'Chester', 'type': 'imaginary'});

    // now set the conversation in motion...
    convo.activate();
});
```

Given the variables defined in this code sample, `foo`, a simple string, `list`, an array, and `object`, a JSON-style object,
the following Mustache tokens and patterns would be available:

```
The value of foo is {{vars.foo}}

The items in this list include {{#vars.list}}{{value}}{{/vars.list}}

The object's name is {{vars.object.name}}.

{{#foo}}If foo is set, I will say this{{/foo}}{{^foo}}If foo is not set, I will say this other thing.{{/foo}}
```
Botkit ensures that your template is a valid Mustache template, and passes the variables you specify directly to the Mustache template rendering system.
Our philosophy is that it is OK to stuff whatever type of information your conversation needs into these variables and use them as you please!

### Built-in Variables

Botkit provides several built in variables that are automatically available to all messages:

{{origin}} - a message object that represents the initial triggering message that caused the conversation.

{{responses}} - an object that contains all of the responses a user has given during the course of the conversation. This can be used to make references to previous responses. This requires that `convo.ask()` questions include a keyname, making responses available at `{{responses.keyname}}`

### Included Utterances

| Pattern Name | Description
|--- |---
| bot.utterances.yes | Matches phrases like yes, yeah, yup, ok and sure.
| bot.utterances.no | Matches phrases like no, nah, nope
| bot.utterances.quit | Matches phrases like, cancel, exit, stop

### Conversation Control Functions

In order to direct the flow of the conversation, several helper functions
are provided.  These functions should only be called from within a convo.ask
handler function!

`convo.sayFirst(message)` Works just like convo.say, but injects a message into the first spot in the queue
so that it is sent immediately, before any other queued messages.

`convo.stop()` end the conversation immediately, and set convo.status to `stopped`

`convo.repeat()` repeat the last question sent and continue to wait for a response.

`convo.silentRepeat()` simply wait for another response without saying anything.

`convo.next()` proceed to the next message in the conversation.  *This must be called* at the end of each handler.

`convo.setTimeout(timeout)` times out conversation if no response from user after specified time period (in milliseconds).

### Handling End of Conversation

Conversations trigger events during the course of their life.  Currently,
only two events are fired, and only one is very useful: end.

Conversations end naturally when the last message has been sent and no messages remain in the queue.
In this case, the value of `convo.status` will be `completed`. Other values for this field include `active`, `stopped`, and `timeout`.

```javascript
convo.on('end',function(convo) {

  if (convo.status=='completed') {
    // do something useful with the users responses
    var res = convo.extractResponses();

    // reference a specific response by key
    var value  = convo.extractResponse('key');

    // ... do more stuff...

  } else {
    // something happened that caused the conversation to stop prematurely
  }

});
```

### Handling Conversation Timeouts

If a conversation reaches its timeout threshold (set using `convo.setTimeout()`) while waiting for a user to respond to a `convo.ask()` question, the conversation will automatically end. By default, the conversation will end immediately without sending any further messages. Developers may change this behavior in one of two ways:

*Provide a handler function with convo.onTimeout():*
Use `convo.onTimeout(handler)` to define a function that will be called when the conversation reaches the timeout threshold. This function
can be used to prevent the conversation from ending, or to take some other action before ending such as using `gotoThread()` to  change the direction of the conversation.

Note that functions used with onTimeout must call `gotoThread()`, `next()`, or `stop()` in order for the conversation to continue.

```
convo.onTimeout(function(convo) {

  convo.say('Oh no! The time limit has expired.');
  convo.next();

});
```

*Provide an `on_timeout` conversation thread:*
Instead of providing a function, developers may choose to specify a pre-defined thread to be used in the case of a timeout event.
This thread should be called `on_timeout`.

```
convo.addMessage('Oh no! The time limit has expired.','on_timeout');
convo.addMessage('TTYL.','on_timeout');
```


### Originating Messages

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


## Botkit Statistics Gathering

As of version 0.4, Botkit records anonymous usage statistics about Botkit bots in the wild.
These statistics are used by the Botkit team at [Howdy](http://howdy.ai) to measure and
analyze the Botkit community, and help to direct resources to the appropriate parts of the project.

We take the privacy of Botkit developers and their users very seriously. Botkit does not collect,
or transmit any message content, user data, or personally identifiable information to our statistics system.
The information that is collected is anonymized inside Botkit and converted using one-way encryption
into a hash before being transmitted.

### Opt Out of Stats

To opt out of the stats collection, pass in the `stats_optout` parameter when initializing Botkit,
as seen in the example below:

```javascript
var controller = Botkit.slackbot({
    stats_optout: true
});
```


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


#### controller.on()
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

#### controller.changeEars()
| Argument | Description
|--- |---
| test_function | a function in the form function(tests_array, message_object)

This function can be used to modify the way Botkit's hearing system works by replacing the pattern matching function with a custom function.

The test function must perform synchronous tests, and should return true if the message represents a match, and otherwise return false.

Code based on Botkit's built-in regular expression test is below:

```
var hears_regexp = function(tests, message) {
    for (var t = 0; t < tests.length; t++) {
        if (message.text) {

            // the pattern might be a string to match (including regular expression syntax)
            // or it might be a prebuilt regular expression
            var test = null;
            if (typeof(tests[t]) == 'string') {
                try {
                    test = new RegExp(tests[t], 'i');
                } catch (err) {
                    botkit.log('Error in regular expression: ' + tests[t] + ': ' + err);
                    return false;
                }
                if (!test) {
                    return false;
                }
            } else {
                test = tests[t];
            }

            if (match = message.text.match(test)) {
                message.match = match;
                return true;
            }
        }
    }
    return false;
};

controller.changeEars(hears_regexp);
```

#### controller.excludeFromConversations()

| Argument | Description
|--- |---
| events | single event or an array of events to exclude from conversations

Messaging platforms send a dizzying array of event types - and new ones are added all the time!

Sometimes, it is desirable to exclude certain event types from inclusion in Botkit conversations. To do this, call `excludeFromConversations()` after creating your controller object.

For example:

```javascript
// always exclude facebook postback events from conversations
controller.excludeFromConversations('facebook_postback')
```

#### controller.spawn()
| Argument | Description
|--- |---
| configuration | an object with instance-specific configuration options
| callback | a function that will receive the new bot instance as a parameter

Spawn a new instance of the bot in order to send messages or handle replies.

Whenever the bot is doing something in response to an incoming message, Botkit will handle spawning bot instances for you.  However, in the event that your bot needs to send an alert or subscription message that is not directly in reply to an incoming message, spawning a bot is required.

The configuration options required to spawn a bot instance differ from platform to platform. In many cases, no additional configuration is required. Refer to the platform specific documnentation for more detail.

```javascript
function sendAlertToBot(alert_message) {

  controller.spawn({}, function(bot) {

    bot.say(alert_message);

  });

}
```

#### controller.defineBot()
| Argument | Description
|--- |---
| bot_constructor | function of the form function(botkit_controller, configuration) which returns a bot instance

This function is used to create new platform connectors for Botkit, and is used to define the specific behaviors and features of that platform's bot instance.

For more information, [read the guide to creating new platform connectors for Botkit](howto/build_connector.md)


#### controller.setTickDelay()
| Argument | Description
|--- |---
| delay | time between event loops

Adjust the speed of Botkit's event loops. By default, this is set to 1500ms, which means that Botkit will evaluate all active conversations and send any pending replies every 1.5 seconds.

Decreasing this time will increase the speed at which bots send replies in conversations.

For example:
``` javascript
// only wait 100ms between conversation loops
controller.setTickDelay(100);
```

#### controller.startTicking()

This function begins Botkit's internal event loop, which will tick at an interval of once every 1.5 seconds unless modified using [controller.setTickDelay()](#controller-settickdelay)

In most cases, this is called internally by a platform connector and does not have to be called directly by bot code.

However, if your bot seems to freeze up after the first message in any conversation, you may need to call it!


#### controller.setupWebserver()
| Argument | Description
|--- |---
| port | a port number for the webserver
| callback | a function in the form function(err, webserver) {...}

Create an instance of the [Express.js webserver](https://expressjs.com) for use with platform-specific features like oauth and incoming webhooks.

Botkit provides a simple Express webserver for those who don't know or don't want to set up their own. However, if you want to do anything with your webserver other than those required by Botkit, it is our recommendation that you manage your own instance of Express. A good place to see this in action is [inside all of the botkit starter kits](starterkits.md).

#### controller.userAgent()

Returns a user agent string for use when making API requests to external resources.

#### controller.version()

Returns the current package version of Botkit's core library

#### controller.shutdown()

This function _stops_ the event loop from processing active conversations. It is the opposite of [controller.startTicking()](#controller-startticking)
