[Bot menus](https://msdn.microsoft.com/en-us/microsoft-teams/botmenu) help to educate your users about critical bot functionality. The menu displays a command with help text you can edit to describe the command's purpose. This information is defined in your bot's manifest file.

![image alt text](image_0.png)

Best practices from Microsoft:

* ####"Keep it simple: the bot menu is meant to present the key 3-5 capabilities or commands of your bot."

* ####"Keep it short: menu options shouldn’t be extremely long and complex natural language statements – they should be simple commands."

* ####"Always available: bot menu actions/commands should be always invokable, regardless of what state of the conversation or dialog the bot is in."

You can define your Bot Menu for Microsoft Teams in the [App Package settings](https://botkit.groovehq.com/knowledge_base/topics/create-an-app-package-for-microsoft-teams). Navigate to the bottom of the page and click "Edit Tabs" to find a view where you can [define your tab settings](https://botkit.groovehq.com/knowledge_base/topics/managing-bot-tabs) for inclusion in your manifest.

## Bot Menu Editor
In your [App Package settings](https://botkit.groovehq.com/knowledge_base/topics/create-an-app-package-for-microsoft-teams) you will see a link to the Bot Menu Editor, which allows you to define two types of Commands:

### Public Commands
These commands will be presented to a user for use in channels.

### Private Commands
These commands are for use in 1:1 chat with your bot.

Each field supports up to 10 commands, but it is recomended that you stick to 1-3 of your most anticipated commands for best user experience.

Each command has a `Title`, which should usually be the trigger for your script, and a `Description`, which is text for your users that describes the function of the script.

These fields come pre-populated with content, you should remove this before exporting your manifest. Click save will take you back to your App Package editor.

### Related Links
* [Microsoft Bot menu documentation](https://msdn.microsoft.com/en-us/microsoft-teams/botmenu)
* [Creating your App Package](https://botkit.groovehq.com/knowledge_base/topics/create-an-app-package-for-microsoft-teams)
* [Creating Static and Configuerable Tabs](https://botkit.groovehq.com/knowledge_base/topics/managing-bot-tabs)

