
When [editing your studio scripts](https://botkit.groovehq.com/knowledge_base/categories/scripts-4) on a Microsoft Teams bot, we support a rich assortment of attachment types to use in [your bot's scripts](https://botkit.groovehq.com/knowledge_base/topics/script-editor).

For more on handling attachments directly in code, see the [working with attachments and media](https://github.com/howdyai/botkit/blob/dev/teams/docs/readme-teams.md#working-with-attachments-and-media) section of the Botkit documentation.

### Supported message formats

[The Microsoft Team's documentation](https://msdn.microsoft.com/en-us/microsoft-teams/botsmessages) provides an in-depth resource to the various types of messages your bot can send. Here are the options currently supported in the Studio script editor:

#### [Hero Cards](https://msdn.microsoft.com/en-us/microsoft-teams/botsmessages#hero-card)

The hero card renders a title, subtitle, text, large image, and buttons. In Studio you have the following fields:

* `Title` = The optional title of the card. 
* `Subtitle` = The optional subtitle of the card.
* `Text` = Optional longer descriptive content you can use for your card.
* `Image` - URL to a 16:9 aspect ratio image you can use in the card.
* `Image Alt` - Text you can (and should) use for clients who cannot use image based cards (for example a user using a reading application).
* `Add Button` - Adds a button with the following attributes: 
	* `Title` - The label of your button
	* `Action` - Provides one of three options to execute when a user clicks on this. Options are: `Say`: has your bot utter the value indicated in the next field; `Open Link`: Opens a given web URL; `Trigger Event`: this will trigger the event specified in the `Value` field.
	* `Value`	- The content that the action will use to peform a given action.
* `Specify tap action` - When activated, allows you to specify an action will be activated when the user taps on the card itself. The following fields are available for use, and work the same as they do in buttons:
	* `Tap Action Title` - The label of your button
	* `Tap Action` - Provides one of three options to execute when a user clicks on this. Options are: `Say`: has your bot utter the value indicated in the next field; `Open Link`: Opens a given web URL; `Trigger Event`: this will trigger the event specified in the `Value` field.
	* `Value`	- The content that the action will use to peform a given action.

#### [Thumbnail Cards](https://msdn.microsoft.com/en-us/microsoft-teams/botsmessages#thumbnail-card)
The thumbnail card renders a title, subtitle, text, small thumbmail image, and buttons. This is very similar to a Hero card, but takes up less screen real estate in your message. 

This type of card is otherwise identical to a standard `Hero Card`.

#### [Image attachments](https://msdn.microsoft.com/en-us/microsoft-teams/botsmessages#picture-messages)

In lieu of a text message, you can send a static image as your message. Pictures can be at most 1024×1024 and 1 MB in PNG, JPEG, or GIF format; animated GIF is not officially supported. This attachment has the following fields:

* `Name` - Text field to describe the card to users.
* `URL` - Public URL to where the image is hosted.
* `File type` - Which of the three supported file types this image is.

### Attachment Layout
If your message contains more than one attachment you will see an option to select one of two layouts:
* `List` - [The list layout](https://msdn.microsoft.com/en-us/microsoft-teams/botsmessages#list-layout) allows you to view the attachments in your message vertically stacked.
* `Carousel` - [The carousel layout](https://msdn.microsoft.com/en-us/microsoft-teams/botsmessages#carousel-layout) allows you to view the attachments in your message horizontally stacked.

### Related Links
* [Working with attachments and media in Botkit](https://github.com/howdyai/botkit/blob/dev/teams/docs/readme-teams.md#working-with-attachments-and-media)
* [Botkit Studio Scripts](https://botkit.groovehq.com/knowledge_base/categories/scripts-4)
* [Microsoft Teams Home](https://botkit.groovehq.com/knowledge_base/categories/microsoft-teams-2)









