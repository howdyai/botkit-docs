### Keep your bot secure and your user messages private!

How do the Botkit tools handle your messages? Where do messages come from and go to?

1. The Botkit-powered application you build (and host yourself) receives messages directly from a messaging platform such as Slack or Facebook.

2. Botkit will evaluate this message locally (within your application) to match it to internally defined triggers (using `hears()` or `on()` handlers).

3. If and only if a message matches the conditions for being analyzed remotely -- by default, only messages sent directly to the bot that are not already part of an ongoing interaction -- the message is sent over an encrypted SSL channel to the Botkit Studio trigger system.

    The trigger system (using [studio.runTrigger](#controllerstudioruntrigger)) will evaluate the input message against all configured triggers, and, should one be matched, return the full content of the script to your application.

4. Botkit will _compile_ the script received from the API into a [conversation object](readme.md#multi-message-replies-to-incoming-messages) and conduct the resulting conversation. During the course of the conversation, *no messages* are sent to the Botkit Studio APIs. Said another way, while a bot is conducting a scripted conversation, all messages are sent and received directly between the application and the messaging platform.  This projects your user's messages, reduces network traffic, and ensures that your bot will not share information unnecessarily.

Our recommended best practices for the security and performance of your bot and the privacy of your users is to send as few messages to be interpreted by the trigger APIs as possible. As described above, a normal Botkit Studio bot should _only_ send messages to the API that can reasonably be expected to contain trigger words.

The bottom line is, Botkit Studio does _not_ put itself between your users and your application. All messages are delivered _first_ and directly to your application, and only sent to our APIs once specific criteria are met. This is notably different than some other bot building services that actually receive and process messages on your behalf.
