Hosting your Botkit project on [Heroku](https://www.heroku.com/) is an easy way to quickly host your bot and program it using Botkit Studio. Heroku is a platform that helps "helps them deliver unique apps in a way that supports their own requirements and development practices." They offer free and paid solutions, and you may find the free service works great for your bot!

[Watch a video of these steps](https://www.youtube.com/watch?v=9Do6QjEP-eY&t=1s)

## Get started
1. Create [your bot](https://botkit.groovehq.com/knowledge_base/topics/create-your-bot)
2. It will be easier to setup your bot if you also set up a new application in your destination service's developer portal simultaneously. [Here is a guide on doing this](https://github.com/howdyai/botkit/tree/master/docs/provisioning).
3. On your bot's [hosting page](https://botkit.groovehq.com/knowledge_base/categories/integrations-58) you will see an option to `Deploy a Bot App`, next to `Heroku` select `Choose`.
4. If you want, Studio can store your tokens created in step 2, it will then use them to automatically setup your Heroku bot. If you'd rather enter them by hand, go ahead and leave these fields blank. Now click `Deploy`.
5. Heroku will automatically remix your own copy of the starter kit.
6. If you entered the platform tokens in Step 3, you can skip to the next step. If not, click on the .env file and enter your:

	* `Platform tokens` (your created these in step 2 above)
	* `Studio Token` =  This is located on the [API KEYS](https://botkit.groovehq.com/knowledge_base/topics/api-keys-1) of your Integrations tab in Botkit Studio.

7. Click on `Show Live` to start your app. If all the .env tokens are correct, You can now go back to Studio. Studio will now provide any information you need to complete step 2, if needed.
8. Now your bot is ready for use!

## Heroku FAQ
*Heroku seems confusing!*

It is quite powerful, so there is a ton of options. For the basic purpose of running a Studio bot, our deployment should contain everything you need to get started, but if you are wondering how to supercharge your usage of this tool, you should check out the [Heroku support documentation](https://www.heroku.com/support) for help in customizing all the server side features of your application.
