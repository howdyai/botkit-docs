The console gives you a full list of all messages sent to your bot, and lets you know whether that resulted in any actionable triggers.

### Opt-in
The first step is to opt-in to console collection by clicking `Enable Console`. once this is done Botkit Studio will capture and display all messages that are sent by your bot for evaluation.

If you to retain data longer than 30 days, you may do so via your [bot settings](https://botkit.groovehq.com/knowledge_base/topics/settings-13)

#### Options
* `Test your triggers with sample input` - Type your messages hear to see what happens when a user utters this message to your bot. This does not count against your API calls.
* `View` - Select ``all` or `unhandled` to filter the console to specific types of messages in the console

## Console Log fields
* `Time` - The timestamp of when this message was received (in CDT/CST).
* `Message Text` - The content of the message received.
* `Trigger` - Indicates if the response matched any of the existing trigger Studio knows, and what script that resulted in.
* `Actions` - You can currently select the following options:
	* `NLP Options` - Select this to use an message as training data for a NLP integration like [Microsoft Luis](https://botkit.groovehq.com/knowledge_base/topics/microsoft-luis)

