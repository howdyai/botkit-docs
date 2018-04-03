![](https://cdn-images-1.medium.com/max/600/1*esI74bsATePATKergb1UJg.png)

A common pattern in bots is collecting information by asking a series of questions, then storing the answers in a user’s profile. This might happen during interactions like the on-boarding of a new bot app, or as part of a profile creation experience when joining a new Slack team.

In this brief developer tutorial, you will learn how to implement this type of functionality in your own bot, using [Botkit Studio](https://studio.botkit.ai/).
In the end, you’ll have a real bot that you can continue to customize and use on your team!

To follow along, you will need:

* [A boilerplate Slack bot, configured through Botkit
Studio](https://studio.botkit.ai/signup?code=medium)
* The example code found in this [Github
repo](https://github.com/howdyai/studio-examples/tree/master/account_profile), which is referenced below

*****

We’re going to build this experience — an account creation flow that will result in a database record being updated with information collected in Slack:

![](https://cdn-images-1.medium.com/max/600/1*xQGNJoE-iN7Q8jY1WIZbHw.gif)

First, we will create a new script to control the account creation conversation. Use Botkit’s import feature to add [this pre-built
script](https://raw.githubusercontent.com/howdyai/studio-examples/master/account_profile/scripts.json).

You can import the script directly from the url, or copy-paste the content of that link into the import dialog. You will then be able to view and modify this script inside Botkit’s dialog editor.

![](https://cdn-images-1.medium.com/max/750/1*-wJa_inKUY5tqZwTOHUYhA.png)

This script has several threads, each defining a piece of the experience:

The  thread asks the user a yes/no question: Do you want to create a profile? If the user says “yes”, the bot will continue on to the  thread, where the actual profile questions are defined.

The  thread contains a series of questions. Each question captures a different value for the user’s profile: an email address, a teeshirt size, and a favorite
color. These questions can, of course, be modified to fit your needs.

Note that a **conditional action** has been attached to the email address
question — in this case, the pattern is a regular expression that will only
match an email address. If the user enters a valid address, this condition will
be met, and the thread will continue. Otherwise, if the user responds with
something else, the bot will repeat the question. **Conditional actions like
this can be used to create all sorts of simple behaviors like this!**

#### Buttons that Talk

<span class="figcaption_hack">Buttons that answer questions</span>

Another clever of Botkit features is demonstrated in the teeshirt size question.
In addition to allowing the user to type their response, interactive message
buttons are also presented with clickable choices, that when clicked, update the
message to reflect the selected response. How does this work?

These buttons are configured in a way that causes Botkit to interpret the clicks
as if the user had typed something. Any time a button has the  “say,” Botkit
will  the  then automatically update the message in Slack. Cool!!

After collecting answers for all the questions in the  thread, the script
transitions to the  thread, which displays a success message and marks the
conversation as successfully completed. This signals to the Botkit backend that
it is time to process the information and store it in the database.

Before we get to that, a note on the other threads present in this script: a 
thread is included in order to handle the cases when a user says that, no, they
aren’t actually interested in updating their profile. This thread marks the
conversation as , which signals to Botkit *not* to process the profile update.

There’s also an  thread. This thread will be used in the event that the user
does not respond to a question within 15 minutes.

With this script enabled, your Botkit bot will be able to respond when you say
“my account,” and will dutifully collect all of your answers. However, those
answers are not yet being stored in a database. **Now, we’ve got to add a little
bit of simple code to our Botkit app!**

#### Add Some 🤖 IQ Points

[Botkit’s
SDK](https://github.com/howdyai/botkit/blob/master/docs/readme-studio.md)
provides a hook for connecting code to the *end* of a conversation. By adding a
simple module to the starter kit application, your bot gains the ability to
receive, process and store the results of the “my account” script described
above.

[Grab the code from this
url](https://github.com/howdyai/studio-examples/blob/master/account_profile/create_account.js)
to add it to your project, or see below. This module does one thing — using , it
adds a function hook to the end of the conversation. After checking that the
conversation ended successfully, it uses Botkit’s built in storage system to
write the user profile data to the database. Voila!

Now, when you start up the bot and say , you will be guided through the
customized profile building process. Upon successful completion, the data will
be stored into a simple database table for use in other parts of the
application. It only took a few lines of actual code to make your bot do
something real!

#### Next Steps:

Hopefully this tutorial will enable you to add new, useful features to your bot.
These techniques can be applied to many different problems your bot might face!
