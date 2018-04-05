Botkit Studio does not host your Botkit application. This advantageous in that you have full control on the data that goes through your bot before it is sent to the platform or to Botkit studio. 

You can either host [Botkit yourself](https://botkit.groovehq.com/knowledge_base/topics/hosting-your-bot-on-your-development-machine), or on any number of Hosting services, for ex. [Glitch](https://botkit.groovehq.com/knowledge_base/topics/hosting-your-bot-on-glitch) or [Heroku](https://botkit.groovehq.com/knowledge_base/topics/hosting-your-bot-on-heroku).

It is not required to have a working Botkit server to create [Botkit Studio scripts](https://botkit.groovehq.com/knowledge_base/categories/scripts-4), but being able to talk to a live bot while your build the function of your bot will greatly aid the bot development process.

## Troubleshooting FAQ
### Why is my bot offline?

There are three basic causes when a bot cannot connect to Botkit Studio:

#### 1. The bot is configured incorrectly in your platform's developer portal. 

 *Solution: Check that your bot is provisioned correctly with the platform* 

We maintain detailed provisioning guides for some of the more common platforms we support on [the offical Github](https://github.com/howdyai/botkit/tree/master/docs/provisioning). These are subject to change on the platform side, so check back here ocassionaly to stay up to date on all the latest changes.

Pay praticular attention to:

* Your bot has all the appropriate permissions and settings it needs from the platform.
* References to your Botkit server endpoints are accurate and spelled correctly.
* If a platform provides validation, use it to check that your Botkit application is running correctly.
 
#### 2. Your Botkit service is not currently running, or missing tokens for the platform and/or Botkit Studio.

*Solution: Check that Botkit is correctly running and has all the needed tokens*
  
Review the [getting started guide]() and make sure Botkit is functioning properly.


#### 3. Your server cannot access the outside world or is not running on a SSL'd connection.

*Solution: Check that your Botkit server can connect to the outside world.*

If you are _not_ running your bot at a public, SSL-enabled internet address, use a tool like [ngrok](http://ngrok.io) or [localtunnel](http://localtunnel.me) to create a secure route to your development application.

If you are running your bot on [Glitch](https://botkit.groovehq.com/knowledge_base/topics/hosting-your-bot-on-glitch) or [Heroku](https://botkit.groovehq.com/knowledge_base/topics/hosting-your-bot-on-heroku), you will be provided everything you need to run Botkit with SSL-enabled internet address.


### Still having trouble?
Join our thriving community of Botkit developers and bot enthusiasts at large. Over 4500 members strong, [our open Slack group](http://community.botkit.ai) is _the place_ for people interested in the art and science of making bots.
Come to ask questions, share your progress, and commune with your peers!

You can also find help from members of the Botkit team [in our dedicated Cisco Spark room](https://eurl.io/#SyNZuomKx)!

If you need to contact us, [please read](https://botkit.groovehq.com/knowledge_base/topics/contact-us-23)

