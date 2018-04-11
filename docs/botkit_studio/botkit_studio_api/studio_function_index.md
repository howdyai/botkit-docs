### controller.studio.run()
| Argument | Description
|---  |---
| bot   | A bot instance
| input_text | The name of a script defined in Botkit Studio
| user | the user id of the user having the conversation
| channel | the channel id where the conversation is occurring

`controller.studio.run()` will load a script defined in the Botkit Studio authoring tool, convert it into a Botkit conversation, and perform the conversation to it's completion.

Developers may tap into the conversation as it is conducted using the [before](#controllerstudiobefore), [after](#controllerstudioafter), and [validate](#controllerstudiovalidate) hooks. It is also possible to bind to the normal `convo.on('end')` event because this function also returns the resulting conversation object via a promise:

```javascript
controller.studio.run(bot, 'hello', message.user, message.channel).then(function(convo) {
    convo.on('end', function(convo) {
        if (convo.status=='completed') {
            // handle successful conversation
        } else {
            // handle failed conversation
        }
    });
});
```

### controller.studio.get()
| Argument | Description
|---  |---
| bot   | A bot instance
| input_text | The name of a script defined in Botkit Studio
| user | the user id of the user having the conversation
| channel | the channel id where the conversation is occurring

`controller.studio.get()` is nearly identical to `controller.studio.run()`, except that instead of automatically and immediately starting the conversation, the function returns it in a dormant state.

While developers may still tap into the conversation as it is conducted using the [before](#controllerstudiobefore), [after](#controllerstudioafter), and [validate](#controllerstudiovalidate) hooks, it must first be activated using `convo.activate()` in the results of the promise returned by the function.

This enables developers to add template variables to the conversation object before it sends its first message. Read about [using variables in messages](readme.md#using-variable-tokens-and-templates-in-conversation-threads)

```javascript
controller.studio.get(bot, 'hello', message.user, message.channel).then(function(convo) {
    convo.setVar('date', new Date()); // available in message text as {{vars.date}}
    convo.setVar('news', 'This is a news item!'); // ailable as {{vars.news}}

    // crucial! call convo.activate to set it in motion
    convo.activate();
});
```


### controller.studio.runTrigger()
| Argument | Description
|---  |---
| bot   | A bot instance
| input_text | The name of a script defined in Botkit Studio
| user | the user id of the user having the conversation
| channel | the channel id where the conversation is occurring

In addition to storing the content and structure of conversation threads, developers can also use Botkit Studio to define and maintain the trigger phrases and patterns that cause the bot to take actions. `controller.studio.runTrigger()` takes _arbitrary input text_ and evaluates it against all existing triggers configured in a bot's account. If a trigger is matched, the script data is returned, compiled, and executed by the bot.

This is different than `studio.run()` and `studio.get()` in that the input text may include _additional text_ other than an the exact name of a script. In most cases, `runTrigger()` will be configured to receive all messages addressed to the bot that were not otherwise handled, allowing Botkit Studio to be catch-all. See below:

```javascript
// set up a catch-all handler that will send all messages directed to the bot
// through Botkit Studio's trigger evaluation system
controller.on('direct_message,direct_mention,mention', function(bot, message) {
    controller.studio.runTrigger(bot, message.text, message.user, message.channel).catch(function(err) {
        bot.reply(message, 'I experienced an error with a request to Botkit Studio: ' + err);
    });
});
```

In order to customize the behavior of scripts triggered using `runTrigger()`, developers define `before`, `after` and `validate` hooks for each script. See docs for these functions below.

Another potential scenario for using `runTrigger()` would be to trigger a script that includes additional parameters that would normally be provided by a user, but are being provided instead by the bot. For example, it is sometimes useful to trigger another script to start when another script has ended.

```javascript
controller.hears(['checkin'], 'direct_message', function(bot, message) {

    // when a user says checkin, we're going to pass in a more complex command to be evaluated

    // send off a string to be evaluated for triggers
    // assuming a trigger is setup to respond to `run`, this will work!
    controller.studio.runTrigger(bot, 'run checkin with ' + message.user, message.user, message.channel);
});
```

#### A note about handling parameters to a trigger

While Botkit does not currently provide any built-in mechanisms for extracting entities or parameters from the input text, that input text remains available in the `before` and `after` hooks, and can be used by developers to process this information.

The original user input text is available in the field `convo.source_message.text`. An example of its use can be see in the [Botkit Studio Starter bot](https://github.com/howdyai/botkit-studio-starter), which extracts a parameter from the help command.

```javascript
controller.studio.before('help', function(convo, next) {

    // is there a parameter on the help command?
    // if so, change topic.
    if (matches = convo.source_message.text.match(/^help (.*)/i)) {
        if (convo.hasThread(matches[1])) {
            convo.gotoThread(matches[1]);
        }
    }

    next();

});
```

### controller.studio.before()
| Argument | Description
|---  |---
| script_name   | The name of a script defined in Botkit Studio
| hook_function | A function that accepts (convo, next) as parameters

Define `before` hooks to add data or modify the behavior of a Botkit Studio script _before_ it is activated. Multiple before hooks can be defined for any script - they will be executed in the order they are defined.

Note: hook functions _must_ call next() before ending, or the script will stop executing and the bot will be confused!

```javascript
// Before the "tacos" script runs, set some extra template tokens like "special" and "extras"
controller.studio.before('tacos', function(convo, next) {

    convo.setVar('special', 'Taco Boats');
    convo.setVar('extras', [{'name': 'Cheese'}, {'name': 'Lettuce'}]);

    next();

});
```


### controller.studio.after()
| Argument | Description
|---  |---
| script_name   | The name of a script defined in Botkit Studio
| hook_function | A function that accepts (convo, next) as parameters

Define `after` hooks capture the results, or take action _after_ a Botkit Studio script has finished executing. Multiple after hooks can be defined for any script - they will be executed in the order they are defined.

Note: hook functions _must_ call next() before ending, or the script will stop executing and the bot will be confused!

```javascript
// After the "tacos" command is finished, collect the order data
controller.studio.after('tacos', function(convo, next) {
    if (convo.status == 'completed') {
        var responses = convo.extractResponses();
        // store in a database
    }
    next();
});
```


### controller.studio.validate()
| Argument | Description
|---  |---
| script_name   | The name of a script defined in Botkit Studio
| variable_name | The name of a variable defined in Botkit Studio
| hook_function | A function that accepts (convo, next) as parameters

`validate` hooks are called whenever a Botkit Studio script sets or changes the value of a variable that has been defined as part of the script.

Note: hook functions _must_ call next() before ending, or the script will stop executing and the bot will be confused!

```javascript
// Validate a "sauce" variable in the "taco" script
// this will run whenever the sauce variable is set and can be used to
// alter the course of the conversation
controller.studio.validate('tacos', 'sauce', function(convo, next) {

    var sauce = convo.extractResponse('sauce');
    sauce = sauce.toLowerCase();

    if (sauce == 'red' || sauce == 'green' || sauce == 'cheese') {
        convo.setVar('valid_sauce', true);
        next();
    } else {
        convo.gotoThread('wrong_sauce');
        next();
    }

});
```

### controller.studio.beforeThread()
| Argument | Description
|---  |---
| script_name   | The name of a script defined in Botkit Studio
| thread_name | The name of a thread defined in Botkit Studio
| hook_function | A function that accepts (convo, next) as parameters

`beforeThread` hooks are called whenever a Botkit Studio script changes from one thread to another.

This works just like [convo.beforeThread()](readme.md#convobeforethread), but operates on the automagically compiled scripts managed by Botkit Studio's IDE.

Note: hook functions _must_ call next() before ending, or the script will stop executing and the bot will be confused!
Allows developers to specify one or more functions that will be called before the thread
referenced in `thread_name` is activated.

`handler_function` will receive the conversation object and a `next()` function. Developers
must call the `next()` function when their asynchronous operations are completed, or the conversation
may not continue as expected.  

Note that if `gotoThread()` is called inside the handler function,
it is recommended that `next()` be passed with an error parameter to stop processing of any additional thread handler functions that may be defined - that is, call `next('stop');`

```javascript
// This example demonstrates how to use beforeThread to capture user input, do an asynchronous operation, then display the results in a new thread
// Imagine a conversation called `search` in which the first action is to collect search terms
// the conversation then transitions to the `results` thread, before which we do the actual search!
controller.studio.beforeThread('search', 'results', function(convo, next) {

  var query = convo.extractResponse('query');
  mySearchQuery(query).then(function(results) {

    convo.setVar('results', results);
    next();

  }).catch(function(err) {

    convo.setVar('error', err);
    convo.gotoThread('error');
    next(err);

  });

});
```