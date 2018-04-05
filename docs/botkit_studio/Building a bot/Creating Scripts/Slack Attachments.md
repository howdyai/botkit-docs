Adding attachments to your Slack messages allow you to ["add more context to a message, making them more useful and effective."](https://api.slack.com/docs/message-attachments).

In the Studio [script editor's message window](https://botkit.groovehq.com/knowledge_base/topics/script-editor) one can manage the following elements by clicking on a given message and clicking `Add Attachment` under `Message Properties`:

### Attachment
*Note: Any message text you specify for a given message will operate as the pretext for your message.*

* Title - The optional header title for your attachment field.
* Text - The content of your attachment field. *Note: You can use [Slack formatting](https://get.slack.help/hc/en-us/articles/202288908-Format-your-messages) in attachments for more styling options.*
* Accent Color - Select from a palette of colors to highlight and visually differenate your attachment from other content.
* Fallback Text - This is an optional (but highly recomended) field you can use to specifiy a unformatted alternative to the field for Slack clients that do not support or use attachments.

Each attachment also can optionally add additional fields and actions.

#### Fields

* Title - Title of the field
* Value - Additional content for the field that can be used by developers in interesting ways to present information to users.
* Size - Selecting short will tell clients this field can be listed side-by-side with other values. This can be useful when displaying multiple fields of information at once.
	
#### Action fields
The actions you provide will be rendered as message buttons to users. More on this can be found [in Slack's documentation](https://api.slack.com/docs/message-buttons)

*  Title - The string that will appear on your button. *Note: Cannot contain markup*.
*  Name - Provide a string to give this specific action a name. The name will be returned to your Action URL along with the message's `callback_id` when this action is invoked. Use it to identify this particular response path. If multiple actions share the same name, only one of them can be in a triggered state. *Example: use `say` and your bot will post a trigger to the conversation using the string specified in `Value` below.*
*  Value - Provide a string identifying this specific action. It will be sent to your Action URL along with the `name` and attachment's `callback_id`. If providing multiple actions with the same name, value can be strategically used to differentiate intent. *Example:  If name value is `say`, and the string here is `hello`, your bot would run the `hello` script.*
*  Style - Change the basic formatting of your button, with the following options:
	* Default — The simple default button view.
	* Primary — Slack has designed this to show that a given button is the preferred response amongst a collection of other possible buttons.
	* Danger — Use this to indicate a button action has dangerous consequences.
* Callback ID - A unique number that slack requires per action field or the button will not function. Make anything up you would like, as long as it is unique.

## Custom Fields
Additionally, one can create a [custom field attachment](https://botkit.groovehq.com/knowledge_base/topics/creating-custom-fields-1).

Custom fields allow you to dynamically insert any field you want into the message object. It will be sent to your bot, and passed along to Slack as if it was a native field.





	
	


