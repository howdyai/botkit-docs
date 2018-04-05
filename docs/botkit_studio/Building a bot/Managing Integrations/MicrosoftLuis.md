# Using Microsoft LUIS with Botkit Studio

[Microsoft LUIS](http://luis.ai) is a natural language processing (NLP) tool that takes free form text input and translates it to specific actions and requests that can be processed by software. This is super useful for bots that receive all sorts of messages from users, but must somehow determine the user’s ultimate *intent*. We've made it easy to use LUIS with Botkit Studio to add sophisticated natural language abilities to your bot.

For more information on LUIS, [check out this overview from Microsoft.](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/home)

To read the story behind this integration, [check out our blog.](https://medium.com/p/4d08d14bec19)

Note: You will need to [log in or create an account at LUIS.ai](https://luis.ai/) to get started.

## NLP Glossary ##

Natural language processing was until recently the domain of researchers and computer scientists. As a result, the language used to describe the tools and processes around it can be difficult to understand! Here are some of the commonly used terms, and how they relate to the Botkit/LUIS integration:

### Utterance

The example messages given to a natural language processing tool in order for it to build its statistical model are frequently referred to as "utterances."  A user’s utterances are translated into intents by the NLP tool.

### Intent 

An intent is a pre-defined action or request that is supplied to the NLP tool. Intents can be actions like "order pizza" or “create subscription,” or they can be more general things like “say thanks” or “who are you?” For each *utterance* processed, LUIS will return one or more intents, along with a *confidence level*.

### Confidence

As sophisticated as it is, LUIS is still making a guess at what each message means. Each guess comes with a confidence level, which is displayed as a percentage. In order to trigger an action in Botkit Studio, LUIS must return an event with a confidence level greater than or equal to 70%.

### Entity

In addition to translating *utterances* into *intents*, LUIS can also extract useful bits of information that embedded in the message. These are called entities, and can be thought of like parameters passed along with the intent. For example, in the message "Subscribe me to the bot builder newsletter," the intent would be “subscribe” along with an entity defining what to subscribe to: “the bot builder newsletter.”

### Training

The process of defining *intents* and associating lots and lots of example *utterances* with them is called *training*. Training has to occur before an NLP tool can be used. When used in the context of LUIS, training also means the explicit action of telling LUIS to update itself with new training data.

### Model

All of the *intents, utterances *and* entities *combined make up your *natural language model*.  Thus, we sometimes describe the entire process as *training an nlp model*.

## What does LUIS do for my bot?

Connecting a LUIS account to Botkit Studio will unlock several new features in the Studio UI:

### Use intents from LUIS as triggers 

Intents that are defined in LUIS can be associated with scripts in Botkit Studio, and used as triggers in the API. This is a big upgrade from using regular expressions or keywords, and allows your bot to understand diverse input.

To use a LUIS intent as a trigger, load a script in the dialog editor, click the "Triggers" button, and select “Intent from LUIS.ai” in the first dropdown field.

### LUIS in the Message Console

As messages are sent to your bot and interpreted by Botkit Studio’s API, they appear in the message console. With the LUIS integration enabled, a new column will appear that displays how LUIS interpreted your message, and allows you to take action in the event that LUIS was not confident enough in its interpretation (or if the interpretation was incorrect.) 

You can see the full results from LUIS by hovering over the "i" icon.

Click the **train** button to use the message as training data to improve your LUIS language model. A dialog will appear that allows you to assign the message to a new or existing intent, as well as identify entities within the message.

Once new data has been added, a widget will appear at the top of the console that allows you to retrain LUIS’ model and make the new training data available for use.

Note: This feature requires the message console to be enabled!

## Configure LUIS within Botkit Studio

To configure LUIS to be used with your bot, navigate to the Integrations page within your bot's settings. You'll find an NLP tab with options.

### Enter Programmatic API Key

You will need a Programmatic API Key from LUIS which identifies your account, and grants Botkit Studio permission to access the information within. Get your key from Microsoft after creating a LUIS account. Copy your [key from this page on the LUIS site](https://www.luis.ai/home/keys) and enter it into the integration page.

### Select an Application Profile

LUIS application profiles define different natural language models. Select an existing application profile from the list displayed, or create a new one to use.

If you choose to create a new application profile, Botkit Studio will pre-populate the model with common intents like "say hello" and “who are you?”  Use these intents to provide responses to these common message types! 

### Select a Subscription Key

It is possible to use LUIS for free, without a subscription key. In this trial mode, you are limited to making 1000 requests to LUIS per month. This is likely plenty for a small scale bot that does not receive a lot of messages. Once 1000 requests are exhausted, Botkit Studio will fallback to keyword or regular expression triggers.

If your bot will be making between 1000 and 10,000 requests, Microsoft offers a generous free tier. However, this does require that a free subscription key is acquired from Microsoft Azure. [Click here for more information about purchasing subscription keys](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/LUIS/pricingtier/S0).

Each request to the Botkit Studio trigger API will make 1 request to LUIS.

# FAQ

## When should I train LUIS?

LUIS will need to be explicitly retrained after any new training data is added. Our recommendation is that you retrain LUIS after identifying and adding new training data. It is not necessary to train after adding each new piece of data - rather, data can be added in batches, and then retrained once per session.

## Why does it say LUIS has low or medium confidence?**

If not enough training data is available, or if a message is very different from all previous training data, LUIS may return a low confidence level. Botkit will only use intents with a score of > 70%. A low confidence means < 50%, and a medium confidence means between 50 and 70%.

To increase the confidence level for an intent, provide more training data!

## How can I correct an inaccurate classification?

Sometimes LUIS misinterprets a message as the wrong intent. If this happens, click the "..." button on the message in the console and select “NLP Options.” From this interface, you can correct LUIS by reclassifying the message with the proper intent. Note that this may not work instantly - multiple examples may be required to correct LUIS.

## How can I use entities?

It is possible to create and identify entities from within the message console, and then use them in scripts once extracted by LUIS.

Currently, there is no representation of the extracted entities within the Botkit Studio UI. However, if they are extracted, they will be available to Botkit as variables within the script, and as responses in the resulting Botkit conversation object.

Given an entity named *subject* that has been extracted from a message, the text value of the entity will be available in the script as `{{vars.subject}}`, and can be used in your Botkit application code by calling `convo.extractResponse(‘subject’)`

## What does the version setting do?

LUIS allows you to create multiple versions of our language model. This is similar to the concept of version control in code - you can create a new version of your model, add new data to it and use it, but still maintain the ability to use (or roll back to) previous versions.  LUIS versions are managed within the LUIS.ai website.

