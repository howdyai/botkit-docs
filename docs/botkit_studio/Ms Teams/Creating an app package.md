## Creating an App Package

In order to sideload your bot for testing in Microsoft Teams, or for submission into the Office 365 store, you will need to create an app package that contains your bot's manifest file. Botkit Studio will create a valid app package for you as part of the standard deploy process, or you can navigate to your Manifest settings to update and download your app package as needed.

The app package is where you define the user experience of your bot in Microsoft Teams. Information like the bot's display name, app description, and icons are outlined and defined here, as well as features like the bot menu and tabs.

### Using Studio to create the App package

Botkit Studio provides tools to create a [Microsoft Teams compatible schema](https://msdn.microsoft.com/en-us/microsoft-teams/schema), the follow fields are available for you to edit on [your bot settings](https://botkit.groovehq.com/knowledge_base/topics/settings-13) page:

#### App manifest
* Microsoft App ID - Created when [provisioning](https://github.com/howdyai/botkit/blob/master/docs/provisioning/teams.md) your bot in [Microsoft's Bot Framework](https://msdn.microsoft.com/en-us/microsoft-teams/botscreate)
* Package name - A unique identifier for this app in reverse domain notation. E.g: com.example.myapp.

#### App information

* App Display Name - The name of your app as it appears in Teams.
* App Full Name - Full name of the app as it appears in the Directory.
* Developer Name - Your Developer name, defaults to your Bot's name.
* Short Description - Simple description of your bot's purpose.
* Long Description - Longer description of your bot's full range of functions.
* Website URL - Website URL for more information about your bot.
* Privacy URL - Privacy policy page on your website, required for submission to the Teams bot directory.
* Terms URL- Terms of service page on your website, required for submission to the Teams bot directory.

#### Icons
Microsoft Teams requires two icons for your app experience, to be used within the product. You can include these in the manifest zip, but we recomend using icons hosted on an external url, as the limitations are less strict. [Read more about icons in this document](https://msdn.microsoft.com/en-us/microsoft-teams/createpackage#icons).

* Outline Icon URL - URL where this file is publically available.
* Full Color Icon URL - URL where this file is publically available.

#### Advanced Editors
The following Teams manifest features have advanced editors that are illustrated more fully in their own support pages:

* [Bot Menu](https://botkit.groovehq.com/knowledge_base/topics/creating-a-bot-menu)
* [Compose Extensions](https://botkit.groovehq.com/knowledge_base/topics/creating-compose-extensions)
* [Tabs](https://botkit.groovehq.com/knowledge_base/topics/managing-bot-tabs)

### Sideloading your App package
Once you have completed adding your bots information, you can click `Save % Download Package` to download a copy of the manifest for [adding to your Teams team](https://msdn.microsoft.com/en-us/microsoft-teams/sideload). If you like to share the bot with others, you can send them a copy of this manifest application .zip, or take the next step of [submitting the bot for inclusion to the Micrsoft Office Bot store](https://msdn.microsoft.com/en-us/microsoft-teams/submission).

#### Related Links:
* "[Create the package for your Microsoft Teams app](https://msdn.microsoft.com/en-us/microsoft-teams/createpackage)" article in the Microsoft Developer Network
* [Creating a Bot Menu](https://botkit.groovehq.com/knowledge_base/topics/creating-a-bot-menu)
* [Creating Static and Configurable Tabs](https://botkit.groovehq.com/knowledge_base/topics/managing-bot-tabs)