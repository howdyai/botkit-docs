# Botkit middlewares

The functionality of Botkit can be extended using middleware functions. These functions can plugin to the core bot running processes at several useful places and make changes to both a bot's configuration and the incoming or outgoing message. Anyone can add their own middleware to the Botkit documentation, [for more information please read this.](#have-you-created-middleware)

Currently the following types of middleware are available for Botkit:
### [Natural language processing](#natural-language-processing)
Natural language processing allows for bots to understand conversational human inputs and interprets your users desires into actionable functions of your application. Once properly trained, these add-ons can dramatically improve the UX for your bot application.

### [Storage Modules](#storage-modules-1)
Storage middleware can be used for storing attributes about a user or channel or team. Botkit supports most major storage methods.

### [Statistics, Metrics, and CRM](#statistics-metrics-and-crm-1)
These plugins help you measure the success of your bots, across a variety of measurable methods.

## Official Botkit Extensions
Name | Project PAge | Info |
|  ------ | ------ | ------ |
| Botkit CMS | https://github.com/howdyai/botkit-cms | An open tool for designing, building and managing interactive dialog systems |


## Natural Language Processing

  Name | Project Page | Info |
|  ------ | ------ | ------ |
|  Microsoft Luis | https://github.com/Stevenic/botkit-middleware-luis | The Luis middleware with Botkit causes every message sent to your bot to be first sent through Luis.ai's NLP services for processing. <br/><br/> |
|  Amazon Lex | https://github.com/jonchurch/botkit-middleware-lex | This middleware allows you to send user input to AWS Lex. This gives you access to intent mapping, data slots, NLP, and other capabilities of the Lex service. You can use this middleware to extend your bots capabilities, add an interaction that lives in Lex to an existing bot, or to run a Lex bot with Botkit as the connector. |
|  Google Dialogflow | https://github.com/jschnurr/botkit-middleware-dialogflow | This middleware plugin for Botkit allows you to utilize Dialogflow (formerly Api.ai), a natural language classifier service directly into the Botkit corebot.<br/><br/> |
|  IBM Watson | https://github.com/watson-developer-cloud/botkit-middleware | This middleware plugin for Botkit allows developers to easily integrate a Watson Conversation workspace with multiple social channels like Slack, Facebook, and Twilio. Customers can have simultaneous, independent conversations with a single workspace through different channels.<br/><br/> |
|  Recast.ai | https://github.com/ouadie-lahdioui/botkit-middleware-recastai | You can use the Recast.AI API to analyse your text or your audio file, and extract useful information from it, to personalize your IoT, classify your data or create bots.<br/><br/> |
|  Wit.ai | https://github.com/howdyai/botkit-middleware-witai | Wit.ai provides a service that uses machine learning to help developers handle natural language input. The Wit API receives input from the user, and translates it into one or more "intents" which map to known actions or choices. The power of Wit is that it can continually be trained to understand more and more responses without changing the underlying bot code!<br/><br/> |
|  Rasa | https://github.com/howdyai/botkit-rasa | This plugin provides Botkit developers a way to use the rasa NLU open source, self hosted natural language API.<br/><br/><br/> |

## Utilities
|  Name | Project Page | Info |
|  ------ | ------ | ------ |
| Janis | https://github.com/Janis-ai/npm-janis | Janis is an AI management toolkit that enables teams working in Slack to collaborate on AI training, monitor bots for problems, and quickly resolve and prevent business-impacting incidents. | 
|  Facebook | https://github.com/pinkku/botkit-middleware-facebookuser | This plugin allows you to collect Facebook Messenger user info to populate a Botkit message |
|  Typing indicator | https://github.com/MarcL/botkit-middleware-typing | This plugin allows you to send the typing indicator for all messages, including Botkit conversations. |

## Storage Modules
|  Name | Project Page | Info |
|  ------ | ------ | ------ |
|  Mongo | https://github.com/howdyai/botkit-storage-mongo | A Mongo storage module for Botkit |
|  Redis | https://github.com/howdyai/botkit-storage-redis | A redis storage module for Botkit |
|  Datastore | https://github.com/fabito/botkit-storage-datastore | A Google Cloud Datastore storage module for Botkit |
|  Firestore | https://github.com/shishirsharma/botkit-storage-firestore | A Firestore storage module for Botkit. |
|  Firebase | https://github.com/howdyai/botkit-storage-firebase | A Firebase storage module for Botkit. |
|  Postgres | https://github.com/lixhq/botkit-storage-postgres | Postgres storage module for Botkit<br/> |
|  CouchDB | https://github.com/mbarlock/botkit-storage-couchdb/ | A Couchdb storage module for botkit |
|  MySQL | https://github.com/getforgex/botkit-storage-mysql/ | A MySQL storage module for botkit |


## Statistics, Metrics, and CRM

|  Name | Project Page | Info |
|  ------ | ------ | ------ |
|  Chatbase | https://github.com/asopinka/chatbase-botkit | A middleware package for Botkit that easily logs your convos in Chatbase|
|  Dashbot Slack | https://www.dashbot.io/sdk/slack/botkit | Increase user engagement, acquisition, and monetization through actionable bots analytics. |
|  Dashbot Facebook | https://www.dashbot.io/sdk/facebook/botkit | Increase user engagement, acquisition, and monetization through actionable bots analytics. |
|  Keen | https://github.com/keen/keen-botkit | This middleware allows you to to understand how many messages are going through your system, run cohorts to measure retention, set up funnels to measure task completion, and any key metric unique to your bot. More information about the Keen platform can be found on their website |
|  Mixpanel | https://github.com/shishirsharma/botkit-mixpanel |This module enables the basic Mixpanel analytics. With this module installed and enabled, your Botkit application will automatically send a copy of any message your bot receives to Mixpanel's analytics API, as well as user profile information. |

# Have you created middleware?
We would love to hear about it! [Contact the Howdy team](https://howdy.ai/) to be included in Botkit documentation, or [submit a PR on this documentation](https://github.com/howdyai/botkit-storage-firebase/blob/master/CONTRIBUTING.md)!
