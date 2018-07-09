# How to build a Botkit Platform Connector

Adding support to Botkit for a new platform requires building a module that gives Botkit the ability to send and receive messages ot the right APIs and in the right format.

Here is an example:

```
module.exports = function(Botkit, config) {

    var controller = Botkit.core(config);
    
    controller.defineBot(function(botkit, config) {

        var bot = {
            type: 'my_platform',
            botkit: botkit,
            config: config || {},
            utterances: botkit.utterances,
        }
        
       
        // here is where you make the API call to SEND a message
        // the message object should be in the proper format already
        bot.send = function(message, cb) {
            console.log('SEND: ', message);
            cb();
        }

        // this function takes an incoming message (from a user) and an outgoing message (reply from bot)
        // and ensures that the reply has the appropriate fields to appear as a reply
        bot.reply = function(src, resp, cb) {
          if (typeof(resp) == 'string') {
            resp = {
              text: resp
             }
          }
          resp.channel = src.channel;
          bot.say(resp, cb);
        }

        // this function defines the mechanism by which botkit looks for ongoing conversations
        // probably leave as is!
        bot.findConversation = function(message, cb) {
            for (var t = 0; t < botkit.tasks.length; t++) {
                for (var c = 0; c < botkit.tasks[t].convos.length; c++) {
                    if (
                        botkit.tasks[t].convos[c].isActive() &&
                        botkit.tasks[t].convos[c].source_message.user == message.user &&
                        botkit.excludedEvents.indexOf(message.type) == -1 // this type of message should not be included
                    ) {
                        cb(botkit.tasks[t].convos[c]);
                        return;
                    }
                }
            }
            cb();
        };

        return bot;

    })


    // provide one or more normalize middleware functions that take a raw incoming message
    // and ensure that the key botkit fields are present -- user, channel, text, and type
    controller.middleware.normalize.use(function(bot, message, next) {

      console.log('NORMALIZE', message);
      next();

    });


    // provide one or more ways to format outgoing messages from botkit messages into 
    // the necessary format required by the platform API
    // at a minimum, copy all fields from `message` to `platform_message`
    controller.middleware.format.use(function(bot, message, platform_message, next) {
        for (var k in message) {
          platform_message[k] = message[k]
        }
        next();
    });


    // provide a way to receive messages - normally by handling an incoming webhook as below!
    controller.handleWebhookPayload = function(req, res) {
        var payload = req.body;

        var bot = controller.spawn({});
        controller.ingest(bot, payload, res);

        res.status(200);
    };


    return controller;

}
```

Now, load your new platform module like this:

```
var controller = require('my_module')(Botkit,{});
```
