Hosting your Botkit project on [Glitch](https://glitch.com/about/) is an easy way to quickly host your bot and test it in Botkit Studio. 

Based on our [Slack Starter Kit](https://github.com/howdyai/botkit-starter-slack), it is one of the easiest ways to standup your studio bot in a production-quality devlopment enviroment while you are building out the functions of your project.

## Get started
1. Create [your bot](https://botkit.groovehq.com/knowledge_base/topics/create-your-bot)
2. On your bot's [hosting page](https://botkit.groovehq.com/knowledge_base/categories/integrations-58) you will see an option to `Deploy a Bot App`, next to `Glitch` select `Choose`.
3. You will see a pop-up with the following settings
		*
		*
		*	

4. Glitch will automatically remix your own copy of the starter kit, at this time you might want to log into glitch using your Github or Facebook logins. This way you can easily find your remixed bot later!
5. You can also rename your bot (although the auto-generated one's Glitch give you tend to be pretty good!)
6. If you entered the Slack tokens in Step 3, you can skip to the next step. If not, click on the .env file and enter your:
		* Clientid = Your Slack id (You received this when [you provisioned your bot]()).
		* ClientSecret = Same as ClientID
		* Studio Token =  This is located on the [API KEYS](https://botkit.groovehq.com/knowledge_base/topics/api-keys-1) of your Integrations tab in Botkit Studio.
7. Your bot is now ready! You can click on the `Show` tab to be taken to your page on Glitch, which will also give you the ability to add the bot to your team!
