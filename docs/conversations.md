# Botkit Conversation Objects


#### convo.activate()

This function will cause a dormant conversation created with [bot.createConversation()](#botcreateconversation) to be activated, which will cause it to start sending messages and receiving replies from end users.

A conversation can be kept dormant in order to preload it with [variables](#using-variable-tokens-and-templates-in-conversation-threads), particularly data that requires asynchronous actions to take place such as loading data from a database or remote source.  You may also keep a conversation inactive while you build threads, setting it in motion only when all of the user paths have been defined.

#### convo.addMessage
| Argument | Description
|---  |---
| message   | String or message object
| thread_name   | String defining the name of a thread

This function works identically to `convo.say()` except that it takes a second parameter which defines the thread to which the message will be added rather than being queued to send immediately, as is the case when using convo.say().

#### convo.addQuestion
| Argument | Description
|---  |---
| message   | String or message object containing the question
| callback _or_ array of callbacks   | callback function in the form function(response_message,conversation), or array of objects in the form ``{ pattern: regular_expression, callback: function(response_message,conversation) { ... } }``
| capture_options |  Object defining options for capturing the response. Pass an empty object if capture options are not needed
| thread_name   | String defining the name of a thread


When passed a callback function, conversation.ask will execute the callback function for any response.
This allows the bot to respond to open ended questions, collect the responses, and handle them in whatever
manner it needs to.

When passed an array, the bot will look first for a matching pattern, and execute only the callback whose
pattern is matched. This allows the bot to present multiple choice options, or to proceed
only when a valid response has been received. At least one of the patterns in the array must be marked as the default option,
which will be called should no other option match. Botkit comes pre-built with several useful patterns which can be used with this function. See [included utterances](#included-utterances)

Callback functions passed to `addQuestion()` receive two parameters - the first is a standard message object containing
the user's response to the question. The second is a reference to the conversation itself.

Note that in order to continue the conversation, `convo.next()` must be called by the callback function. This
function tells Botkit to continue processing the conversation. If it is not called, the conversation will hang
and never complete causing memory leaks and instability of your bot application!

The optional third parameter `capture_options` can be used to define different behaviors for collecting the user's response.
This object can contain the following fields:

| Field | Description
|--- |---
| key | _String_ If set, the response will be stored and can be referenced using this key
| multiple | _Boolean_ if true, support multi-line responses from the user (allow the user to respond several times and aggregate the response into a single multi-line value)

##### Using conversation.addQuestion with a callback:

```javascript
controller.hears(['question me'], 'message_received', function(bot,message) {

  // start a conversation to handle this response.
  bot.startConversation(message,function(err,convo) {

    convo.addQuestion('How are you?',function(response,convo) {

      convo.say('Cool, you said: ' + response.text);
      convo.next();

    },{},'default');

  })

});
```

##### Using conversation.addQuestion with an array of callbacks:

```javascript
controller.hears(['question me'], 'message_received', function(bot,message) {

  // start a conversation to handle this response.
  bot.startConversation(message,function(err,convo) {

    convo.addQuestion('Shall we proceed Say YES, NO or DONE to quit.',[
      {
        pattern: 'done',
        callback: function(response,convo) {
          convo.say('OK you are done!');
          convo.next();
        }
      },
      {
        pattern: bot.utterances.yes,
        callback: function(response,convo) {
          convo.say('Great! I will continue...');
          // do something else...
          convo.next();

        }
      },
      {
        pattern: bot.utterances.no,
        callback: function(response,convo) {
          convo.say('Perhaps later.');
          // do something else...
          convo.next();
        }
      },
      {
        default: true,
        callback: function(response,convo) {
          // just repeat the question
          convo.repeat();
          convo.next();
        }
      }
    ],{},'default');

  })

});
```

#### convo.say()
| Argument | Description
|---  |---
| message   | String or message object

convo.say() is a specialized version of `convo.addMessage()` that adds messages to the _current_ thread, essentially adding a message dynamically to the conversation. This should only be used in simple cases, or when building a conversation with lots of dynamic content. Otherwise, creating `threads` is the recommended approach.

Call convo.say() several times in a row to queue messages inside the conversation. Only one message will be sent at a time, in the order they are queued.

```javascript
controller.hears(['hello world'], 'message_received', function(bot,message) {

  // start a conversation to handle this response.
  bot.startConversation(message,function(err,convo) {

    convo.say('Hello!');
    convo.say('Have a nice day!');

  });
});
```

#### convo.ask()
| Argument | Description
|---  |---
| message   | String or message object containing the question
| callback _or_ array of callbacks   | callback function in the form function(response_message,conversation), or array of objects in the form ``{ pattern: regular_expression, callback: function(response_message,conversation) { ... } }``
| capture_options | _Optional_ Object defining options for capturing the response

convo.ask() is a specialized version of `convo.addQuestion()` that adds messages to the _current_ thread, essentially adding a message dynamically to the conversation. This should only be used in simple cases, or when building a conversation with lots of dynamic content. Otherwise, creating `threads` is the recommended approach.

In particular, we recommend that developers avoid calling `convo.ask()` or `convo.say()` inside a callbacks for `convo.ask()`. Multi-level callbacks encourage fragile code - for conversations requiring more than one branch, use threads!


#### convo.gotoThread()
| Argument | Description
|---  |---
| thread_name   | String defining the name of a thread

Cause the bot to immediately jump to the named thread.
All conversations start in a thread called `default`, but you may switch to another existing thread before the conversation has been activated, or in a question callback.

Threads are created by adding messages to them using `addMessage()` and `addQuestion()`

```javascript
// create the validation_error thread
convo.addMessage('This is a validation error.', 'validation_error');
convo.addMessage('I am sorry, your data is wrong!', 'validation_error');

// switch to the validation thread immediately
convo.gotoThread('validation_error');
```


#### convo.transitionTo
| Argument | Description
|---  |---
| thread_name   | String defining the name of a thread
| message   | String or message object

Like `gotoThread()`, jumps to the named thread. However, before doing so,
Botkit will first send `message` to the user as a transition. This allows
developers to specify dynamic transition messages to improve the flow of the
conversation.

```javascript
// create an end state thread
convo.addMessage('This is the end!', 'the_end');

// now transition there with a nice message
convo.transitionTo('the_end','Well I think I am all done.');
```

#### convo.beforeThread
| Argument | Description
|--- |---
| thread_name | String defining the name of a thread
| handler_function | handler in the form function(convo, next) {...}

Allows developers to specify one or more functions that will be called before the thread
referenced in `thread_name` is activated.

`handler_function` will receive the conversation object and a `next()` function. Developers
must call the `next()` function when their asynchronous operations are completed, or the conversation
may not continue as expected.  

Note that if `gotoThread()` is called inside the handler function,
it is recommended that `next()` be passed with an error parameter to stop processing of any additional thread handler functions that may be defined - that is, call `next('stop');`

```javascript
// create a thread that asks the user for their name.
// after collecting name, call gotoThread('completed') to display completion message
convo.addMessage({text: 'Hello let me ask you a question, then i will do something useful'},'default');
convo.addQuestion({text: 'What is your name?'},function(res, convo) {
  // name has been collected...
  convo.gotoThread('completed');
},{key: 'name'},'default');

// create completed thread
convo.addMessage({text: 'I saved your name in the database, {{vars.name}}'},'completed');

// create an error  thread
convo.addMessage({text: 'Oh no I had an error! {{vars.error}}'},'error');


// now, define a function that will be called AFTER the `default` thread ends and BEFORE the `completed` thread begins
convo.beforeThread('completed', function(convo, next) {

  var name = convo.extractResponse('name');

  // do something complex here
  myFakeFunction(name).then(function(results) {

    convo.setVar('results',results);

    // call next to continue to the secondary thread...
    next();

  }).catch(function(err) {
    convo.setVar('error', err);
    convo.gotoThread('error');
    next(err); // pass an error because we changed threads again during this transition
  });

});
```


#### convo.setVar
| Argument | Description
|---  |---
| variable_name   | The name of a variable to be made available to message text templates.
| value | The value of the variable, which can be any type of normal Javascript variable

Create or update a variable that is available as a Mustache template token to all the messages in all the threads contained in the conversation.

The variable will be available in the template as `{{vars.variable_name}}`

#### convo.onTimeout()
| Argument | Description
|--- |---
| callback | _Optional_ Callback in the form function(convo) { ... }

Provide a handler function that will be called in the event that a conversation reaches its timeout threshold without any user response.


#### convo.extractResponses()

Returns an object containing all of the responses a user sent during the course of a conversation.

```javascript
var values = convo.extractResponses();
var value = values.key;
```

#### convo.extractResponse()

Return one specific user response, identified by its key.

```javascript
var value  = convo.extractResponse('key');
```


`convo.sayFirst(message)` Works just like convo.say, but injects a message into the first spot in the queue
so that it is sent immediately, before any other queued messages.

`convo.stop()` end the conversation immediately, and set convo.status to `stopped`

`convo.repeat()` repeat the last question sent and continue to wait for a response.

`convo.silentRepeat()` simply wait for another response without saying anything.

`convo.next()` proceed to the next message in the conversation.  *This must be called* at the end of each handler.

`convo.setTimeout(timeout)` times out conversation if no response from user after specified time period (in milliseconds).


#### convo.hasThread()

#### convo.successful()
