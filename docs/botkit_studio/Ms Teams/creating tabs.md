
Tabs allow you to display web content right alongside your bot in Microsoft Teams. For more detailed information, see the ["Getting started with tabs for Microsoft Teams"](https://msdn.microsoft.com/en-us/microsoft-teams/tabs) article in the Microsoft Dev Center.

[Using the App Package builder in Botkit Studio](https://botkit.groovehq.com/knowledge_base/topics/create-an-app-package-for-microsoft-teams), you can define the URLs for the Static and Configurable Tabs for inclusion in your manifest. Click the "Edit Tabs" button at the bottom of the App Package page to define your tab information. 

Microsoft provides some suggestions for [creating a great tab](https://msdn.microsoft.com/en-us/microsoft-teams/design#designing-a-great-tab).

For integrating with Tabs in your Botkit code, read more in our [Botkit documentation](https://git.botkit.ai/benbrown/teamsbot/blob/teams/docs/readme-teams.md#using-tabs).


### Configurable Tab
These is where a user configures the content of your tab experience when the tab is first added to a channel. Currently only one such tab is supported, to add it, click `Add Tab`.

Once enabled you have the following options:

* `Configuration Url` - [The url of your configurable tab](https://msdn.microsoft.com/en-us/microsoft-teams/createconfigpage). If you are using a starter kit, we provide a template for you!
* `Can Update Configuration` - Allow users to change any settings they may find on this page.


### Static Tab
A static tab is a static web page you can provide users information to aid in use of the bot (or really any content you can think of!).

Microsoft has a [thorough guide to these fields here](https://msdn.microsoft.com/en-us/microsoft-teams/statictab).

### Valid Domains
All URLs you add in static tabs must be referenced in the validDomains section of the manifest. Failure to do so could result in a blank tab. Click `Add Domain` for whatever static tabs urls you have linked to. You can use domain wildcards to whitelist an entire domain.