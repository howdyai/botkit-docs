## Message Window
The Botkit Studio script editor provides a convenient interface to edit your conversations. [Depending on which platform you chose for your bot](https://botkit.groovehq.com/knowledge_base/topics/manage-your-bots) you will see a platform specific interface that mocks how your bot conversation will appear to end users. 

Each script can be made up be made up of any number of [threads](https://botkit.groovehq.com/knowledge_base/topics/script-threads). 

When you first create a new script, it will come prepoulated with an example message. You can click on this in the message window to edit it. or add a new line using the field below it.

## Editting a message
When you edit a message, you will see the contents of your message in the Message properties pane. Here you can write the content of your messages, formatted using platform specific formatting strings as well as information captured through the process of your conversation.

You also have the ability to create any number of alternate text strings to create variety in your script conversations.

Depending on the primary platform you have selected you may be able to add attachments to each message field. Studio currently supports [Facebook](https://botkit.groovehq.com/knowledge_base/topics/facebook-attachments) or [Slack](https://botkit.groovehq.com/knowledge_base/topics/slack-attachments) type message attachements.

## Capturing user's typed responses
Checking this field allows you to collect information from users and use it for any number of purposes. When checked, it will create a new field in message properties that allows you to collect the content of a user's response into a [variable](https://botkit.groovehq.com/knowledge_base/topics/variables) for use elsewhere in your script.

## Finishing up
When you reach the end of the thread, you can either record an action (`Sucessful`, `Failed`, or `Timed out`0, or move to another [Thread](https://botkit.groovehq.com/knowledge_base/topics/script-threads)in the script.