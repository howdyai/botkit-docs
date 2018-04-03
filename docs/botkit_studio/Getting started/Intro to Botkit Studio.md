[Botkit Studio](https://studio.botkit.ai) is a hosted development tool that enhances and expands the capabilities of Botkit. While developers may use Botkit without Studio, a Studio account will substantially ease the development and deployment of a Bot, help to avoid common coding pitfalls,
a valuable management interface for the bot's dialog content and configuration. Botkit Studio is a product of [Howdy.ai](http://howdy.ai), the creators of Botkit.

This document covers the Botkit Studio SDK details only. [Start here](readme.md) if you want to learn about to develop with Botkit.

For bot developers who have existing apps but would like to benefit from features like bot-specific content management without using Botkit, you can access the [Botkit Studio SDK here](https://github.com/howdyai/botkit-studio-sdk)

## Key Features
### Why use Botkit Studio?
The goal of Botkit Studio is to make building bots with Botkit easier than ever before.
The tools and this SDK are based on feedback from dozens of Botkit developers and more than a year of building and operating our flagship bot, [Howdy](https://howdy.ai).
However, our intent is not to cover every base needed for building a successful bot. Rather, we focus on several key problem areas:

* **Script Authoring**: Botkit Studio provides a cloud-based authoring tool for creating and maintaining multi-thread conversations and scripts. Scripts managed in Studio can be changed without changing the underlying application code, allowing for content updates, feature additions and product development without code deploys. Many types of conversation will _no longer require code_ to function.

* **Trigger Management**: Developers may define trigger words and patterns externally from their code, allowing those triggers to expand and adapt as humans use the bot.

* **Easy to use SDK**: Our open source SDK provides simple mechanisms for accessing these features from within your application.  With only a few new functions: [studio.get](#controllerstudioget), [studio.run](#controllerstudiorun), and [studio.runTrigger](#controllerruntrigger), your bot has full access to all of the content managed by the cloud service. These functions can be used to build light weight integrations with Studio, or turn over complete control to the cloud brain.


### [Function Index](https://botkit.groovehq.com/knowledge_base/topics/studio-api)