![](https://cdn-images-1.medium.com/max/600/1*SBNQp3Dn7yqZ1Gy5Br8NIw.png)

One of the super useful things a bot can do is quickly look up information for a
user, and send the results as a response. Imagine if you had a bot that could
look up bug reports for you, or a bot that can check the status of orders for
your customers. Most of these features follow a similar pattern, one that we’ll
demonstrate how to build yourself in this tutorial.

You will learn how to implement a simple data-powered conversation in your own
bot, using [Botkit Studio](https://studio.botkit.ai/). In the end, you’ll have a
real bot that you can continue to customize and use on your team!

To follow along, you will need:

* [A boilerplate Slack bot, configured through Botkit
Studio](https://studio.botkit.ai/signup?code=medium)
* The example code found in this [Github
repo](https://github.com/howdyai/studio-examples/tree/master/order_status),
which is referenced below

*****

Here’s what it’ll look like in Slack after we’ve built our new command — an
order status feature that looks up an order based on a user’s email address, and
presents a rich card with order information pulled from a dynamic source.

![](https://cdn-images-1.medium.com/max/600/1*nlsnLnMN_m1_XCo-Yk_HyQ.gif)

First, we will create a new script to provide structure and flow to the order
status conversation. Use Botkit’s import feature to add [this pre-built
script](https://raw.githubusercontent.com/howdyai/studio-examples/master/order_status/scripts.json).
You can import the script directly from the url, or copy-paste the content of
that link into the import dialog. You will then be able to view and modify this
script inside Botkit’s dialog editor.

You’ll notice the script has several threads, listed on the left of the dialog
editor. These threads define different elements of the experience. We’ll focus
on the two most important ones for now.

The  is where the conversation will start. In this instance, the bot offers up a
quick greeting, then asks the user for their email address.

After receiving the address, the bot will transition to the  thread, which
includes a rich card with information about the order, and some additional
buttons for taking related actions.

<span class="figcaption_hack">{{mustache.tags}} allow dynamic content to be used in dialog</span>

In the  thread, in the attachment builder, you’ll see lots of weird values that
look like {{vars.this}}. Those are [Mustache template
tags](https://mustache.github.io/) that tell Botkit to put dynamic values into
the script. **Every single field in a Botkit script can use this type of
template tag **to incorporate dynamic content into the dialog.

Without any new code in your bot, this script can be triggered in Slack, and
your bot will dutifully walk through the dialog. However, the resulting order
status card will be blank, because we haven’t actually done the database call
yet.

**How do we replace those template variables with external data?**

#### Add Some Code

Now, we’ll add some code to the bot that does some real work — loading up an
order out of the database, and providing the order details to Botkit.

Whenever a user answers a question, Botkit will look for functional hooks
configured using the  function. This function allows developers to specify a
function tied to a specific variable in a specific script.

[Grab the code from this url
](https://github.com/howdyai/studio-examples/blob/master/order_status/order_status_skill.js)to
add it to your project (also shown below). This module defines a validation
function connected to the  variable, and uses the value to look up an order in
the database. (In our sample code, we’re faking the database lookup, but it
should be easy to swap our sample code out for a real DB or API call).

Then, once the data has been retrieved from the external source, we use  to add
the results into the conversation. This data-binding turns all those places we
have *{{mustache.variables}}* in our script into their matching, dynamically
retrieved values from the database.

In the case that no order is found in the database, the bot transitions to the 
thread, which explains the error, and helps the user try again.

The next time we say  to our bot, we’ll be guided through the conversation. As
long as a valid email address is provided, the  function will be called, and a
dynamic order record will be inserted into the conversation. Now, instead of
{{mustache.variables}}, our rich card in Slack will be filled with real
information!

#### Next Steps:

Hopefully this tutorial will enable you to add new, useful features to your bot.
These techniques can be applied to many different problems your bot might face!

Here are some next steps you can take:

* Join us in the [official Botkit community](http://community.botkit.ai/) to
discuss your projects, talk to other developers, and get help directly from the
Botkit team.
* Connect your own database or API to a conversation, and display dynamic values
in a rich card interface
* Try using the  hook to add variables or transitions based on parameters passed
to your script instead of answers collected from questions

* [Bots](https://blog.howdy.ai/tagged/bots?source=post)
* [Slack](https://blog.howdy.ai/tagged/slack?source=post)
