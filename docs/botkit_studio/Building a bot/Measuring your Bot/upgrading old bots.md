If you have created a bot using a version of [Botkit below version 1.0 ](https://github.com/howdyai/botkit/blob/master/changelog.md), you will need to make two small changes to your project to support the full features of Botkit Studio Metrics:

### Update your package.json
Add the following line to your package.json file:

``` javascript
"botkit-studio-metrics": "git://github.com/howdyai/botkit-studio-metrics.git",
```

### Update your bot.js file
Add the following line to your bot.js file (or whatever you have named your bot project):

``` javascript
require('botkit-studio-metrics')(controller,{debug:true,always_update:true});
```
 
 _Note: If you are creating a new bot, or starting from a new starter kit, the above process should not be necessary._