If you have created a bot using a version of Botkit below version 0.6.5, you will need to make three small changes to your project to support the full features of Botkit Studio Metrics:

_Note: please perform these steps in the order perscribed_

### 1. Upgrade to the latest version of Botkit
Inside your Botkit application's root folder, run the command:

```
npm update --save botkit
```
This will cause your version of Botkit to be updated to the latest available version, and your package.json file will be modified.

Alternately, you can edit your package.json file by hand, changing the version option for Botkit to '^0.6.5'.

### 2. Install Botkit Studio Metrics plugin

Inside your Botkit app, run the following command:

``` javascript
npm install --save botkit-studio-metrics
```

OR, edit your `package.json` file manually and add the following line:


``` javascript
"botkit-studio-metrics": "^0.0.1",
```

### 3. Update your bot.js file

Add the following line to your bot.js file (or whatever you have named your bot project):

``` javascript
require('botkit-studio-metrics')(controller);
```

_Note: If you are creating a new bot, or starting from a new starter kit, the above process should not be necessary._