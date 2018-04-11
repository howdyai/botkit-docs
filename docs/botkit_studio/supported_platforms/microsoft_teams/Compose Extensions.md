One of the unique features of Microsoft Teams is support for "[compose extensions](https://msdn.microsoft.com/en-us/microsoft-teams/composeextensions)" - custom UI elements that appear adjacent to the "compose" bar in the Teams client that allow users to create cards of their own using your bot application.

With a compose extension, you can offer users a way to search or create content in your application which is then attached to their message. Compose extensions can live in both multi-user team chats, as well as 1:1 discussions with the bot.

They work sort of like web forms - as a user types a query, the compose extension API retrieves results from the application and displays them in the teams UI. When a result is selected, a custom app-defined card is attached to the user's outgoing message. 

You can enable compose extensions on your bot project via the enabling it in your manifest file using the [App Package editor](https://botkit.groovehq.com/knowledge_base/topics/create-an-app-package-for-microsoft-teams) and adding some [helper code to your Botkit application](https://github.com/howdyai/botkit/tree/master/docsreadme-teams.md#using-compose-extensions)

### Configure in Studio
The following fields are editable in the [App Package editor](https://botkit.groovehq.com/knowledge_base/topics/create-an-app-package-for-microsoft-teams):

* `Enable Compose Extension` - Toggle this to add the feature to your manifest on export.
* Context selection: You can check the following to enable the exstenion in either: 
	* `Show in Team context` (in shared channels)
	* `Show in 1:1 context` (for direct messaging with the bot)

There are also the following options *(for most users you will want to leave them at their defaults)*:	

* `ID`: (a unique identifier for this extension) 
* `Title`: Command Title
* `Description`: Description of command
*  `Run automatically when extension is activated`: Toggle this if you want the bot to start working without any user input.

Finally, you can define the following parameters:
* `Name` with the default `query`
* `Title` with the default `Search`
* `Desc` with the default `Search by keyword`

### Related Links
* [Creating an App Package](https://botkit.groovehq.com/knowledge_base/topics/create-an-app-package-for-microsoft-teams)
* [Botkit Studio Scripts](https://botkit.groovehq.com/knowledge_base/categories/scripts-4)
* [Microsoft Teams Home](https://botkit.groovehq.com/knowledge_base/categories/microsoft-teams-2)

