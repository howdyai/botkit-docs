Hosting your Botkit project on [Glitch](https://glitch.com/about/) is an easy way to quickly host your bot and test it in Botkit Studio. For many users, it may be all you need to run a production ready bot for your team! 

[Watch a video of these steps]()

## Get started
1. Create [your bot](https://botkit.groovehq.com/knowledge_base/topics/create-your-bot)
2. It will be easier to setup your bot if you also set up a new application in your destination service's developer portal simultaneously. [Here is a guide on doing this](https://github.com/howdyai/botkit/blob/master/docs/provisioning/teams.md).
3. On your bot's [hosting page](https://botkit.groovehq.com/knowledge_base/categories/integrations-58) you will see an option to `Deploy a Bot App`, next to `Glitch` select `Choose`.
4. If you want, Studio can store your tokens created in step 2, it will then use them to automatically setup your Glitch bot. If you'd rather enter them by hand, go ahead and leave these fields blank. Now click `Deploy`.
5. Glitch will automatically remix your own copy of the starter kit, at this time you might want to log into glitch 
6. If you entered the platform tokens in Step 3, you can skip to the next step. If not, click on the .env file and enter your:

	* `Platform tokens` (your created these in step 2 above)
	* `Studio Token` =  This is located on the [API KEYS](https://botkit.groovehq.com/knowledge_base/topics/api-keys-1) of your Integrations tab in Botkit Studio.

7. Click on `Show Live` to start your app. If all the .env tokens are correct, You can now go back to Studio. Studio will now provide any information you need to complete step 2, if needed.
8. Now your bot is ready for use!

## Glitch FAQ
*Why should I log into Glitch?*

We recomend that you sign into Glitch using your Github logins. This way you can easily find your remixed bot later! 

*What are benefits of creating a Glitch account?*
Aside from easily being able to find your project, you can control if the project is public or private, invite collaborators, and tweak some personal user settings. Read more about this on [Glitch's FAQ](https://glitch.com/faq)

*Can I rename my bot?*

You can rename your bot by clicking on its name in the Glitch UI and changing it to one of your choosing (although the auto-generated one's Glitch give you tend to be pretty good!). Please note that changing it's name also changes it's Endpoint URL, so you will need to update your urls in the Platform's developer portal. You will also need to refresh your `Show Live` page to sync the change back to Studio.