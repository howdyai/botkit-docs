Currently Studio supports the following types of [Botkit Anywhere attachments](https://github.com/howdyai/botkit-starter-web)
	
## Text
In the text box, one can type in plain-text, or in [Markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet). Markdown render text will be rendered correctly in [your server's chat ui](https://github.com/howdyai/botkit-starter-web/blob/master/docs/botkit_web_client.md#customize-the-look-and-feel-of-the-chat-interface).

Responses can optionally be captured as variables, more on using these in your scripts can be [found here](https://botkit.groovehq.com/knowledge_base/topics/variables).


## Quick replies

Quick Replies are buttons you can present users to trigger bot interactions without having to type. This allows you to simplify the conversation flow with buttons based on the most expected response to your messages.

In the Studio editor, these can be set per [message](https://botkit.groovehq.com/knowledge_base/topics/script-messages), allowing for great variety in your bot's interface.


### Fields

* `Title` - This is the text that will appear in your button.
* `Payload` - This is the [trigger](https://botkit.groovehq.com/knowledge_base/topics/triggers) that will be sent back to botkit to initiate scripts
	
## Attached files
Here you can attach any filetype to be presented to users as part of your chat. Image filetypes will be displayed in their native size, any other attachment will be presented as a link.

## Custom Fields
Additionally, one can create a [custom field attachment](https://botkit.groovehq.com/knowledge_base/topics/creating-custom-fields-1).

Custom fields allow you to dynamically insert any field you want into the message object. It will be sent to your bot, and passed along to the web chat as if it was a native field.

