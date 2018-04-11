[Botkit Studio](https://studio.botkit.ai) is a hosted development tool for bot builders. Botkit Studio will substantially ease the development and deployment of a Bot, help to avoid common coding pitfalls,
and provide a valuable management interface for the bot's dialog content and configuration. Botkit Studio is a product of [Howdy.ai](http://howdy.ai), the creators of Botkit.

This SDK allows developers to use the Botkit Studio APIs _without using Botkit_.
This is useful for bot developers who have existing apps but would benefit from features like bot-specific content management.

### Installation 

* [Install the SDK from npm:](https://github.com/howdyai/botkit-studio-sdk/blob/master/README.md#install)

```
npm install --save botkit-studio-sdk
```

* [Register for a developer account with Botkit Studio](https://studio.botkit.ai) 
* [Acquire an API token](). This will identify your bot and grant your application
access to the APIs.
* In your bot application, [include the library and create an API client:](https://github.com/howdyai/botkit-studio-sdk/blob/master/README.md#connecting-to-botkit-studio)

```javascript
var BKS = require('botkit-studio-sdk');
var bks_client = new BKS({
    studio_token: 'studio token from botkit studio goes here'
});
```

For the latest version of this document [please view the readme](https://github.com/howdyai/botkit-studio-sdk/blob/master/README.md#connecting-to-botkit-studio).

Updated 12/7/16 

