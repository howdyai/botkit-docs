Currently Studio supports the following types of [Facebook Messenger attachments](https://developers.facebook.com/docs/messenger-platform/send-api-reference)
	
## Buttons
Buttons can provide the following actions in Botkit Studio:

### Postback
When a Postback Button is tapped, Facebook will send a call to your webhook. The following options are available:

* Title - String that will appear on your button.
* Payload - The call you want sent to your webhook.

### Url
The URL Button can be used to open a web page in the in-app browser.
The following options are available:

* Title - String that will appear on your button.
* Url - Link to the website you'd like to open
* Webview size - The size of the in-app browser page you'd like to open. The sizes are:
	* Full
	* Compact
	* Tall

For more details on designing a webpage to exploit this view, read the [Facebook documentation](https://developers.facebook.com/docs/messenger-platform/send-api-reference/url-button)	

### Call
This will allow your bot to send a button with a callback button users can use to contact you.

* Title - String that will appear on your button.
* Number - The phone number.


## Quick replies
Per the [Facebook developer portal](https://developers.facebook.com/docs/messenger-platform/send-api-reference/quick-replies), "Quick Replies provide a new way to present buttons to the user". This allows you to simplify the conversation flow with buttons based on the most expected response to your messages.

In the Studio editor, these can be set per [message](https://botkit.groovehq.com/knowledge_base/topics/script-messages), allowing for great variety in your bot's interface.

One message can have up to 11 quick reply objects.

### Fields
* Type: Text - Selecting this option will create a button with the following attributes
	* Title - This is the text that will appear in your button.
	* Payload - This is the [trigger](https://botkit.groovehq.com/knowledge_base/topics/triggers) that will be sent back to botkit to initiate scripts
	* Image url - Optionally you can use a an image no smaller than 24x24px (think like a custom emoji) to make your bot's interface more fun.
* Type: Location - This will request a user's location data, which you can then use as data for your bot.

## Custom Fields
Additionally, one can create a [custom field attachment](https://botkit.groovehq.com/knowledge_base/topics/creating-custom-fields-1).

Custom fields allow you to dynamically insert any field you want into the message object. It will be sent to your bot, and passed along to Facebook as if it was a native field.

