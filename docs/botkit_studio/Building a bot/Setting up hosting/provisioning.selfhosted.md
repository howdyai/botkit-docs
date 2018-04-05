#Self-Hosted

You can choose any deployement method for hosting your bot. Follow the steps below to get your hosted bot instance talking to Botkit Studio. 

## Get started
1. [Create your bot](https://botkit.groovehq.com/knowledge_base/topics/create-your-bot)
2. It will be easier to setup your bot if you also set up a new application in your destination service's developer portal simultaneously. [(Instructions here)](https://github.com/howdyai/botkit/tree/master/docs/provisioning).
3. On your bot's [hosting page](https://botkit.groovehq.com/knowledge_base/categories/integrations-58) you will see an option to `Deploy a Bot App`, next to `Self-Hosted` select `Choose`.
4. If you want, Studio can store your tokens created in step 2, it will then use them to automatically setup your environment variables. If you'd rather enter them by hand, go ahead and leave these fields blank. Now click `Deploy`.
5. Install Botkit from NPM or GitHub. ([Instructions here.](https://github.com/howdyai/botkit#install-botkit-from-npm-or-github))
6. Run your bot instance on your server using the platform environment variables and your Botkit Studio API Token. This is located in the [API keys](https://botkit.groovehq.com/knowledge_base/topics/api-keys-1) section of your Integrations tab in Botkit Studio.
7. Now your bot is ready for use! Return to Botkit Studio to edit your bot's scripts.

