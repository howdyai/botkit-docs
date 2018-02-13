# Botkit Controllers



## Methods


### hears(keywords, events, middleware_or_cb, cb)

### on(event, cb, is_hearing)

### trigger(event, data)

### hears_regexp(tests, message)

### changeEars(new_test)

### excludeFromConversations(event)

### ingest(bot, payload, source)
### spawn(config, cb)
### defineBot(unit)
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
