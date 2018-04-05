Botkit studio provides two methods for adding and export external scripts to your Bot from your bot page:

## Select
Clicking this will create a view where you can expert your entire inventory of scripts in JSON format. You can unselect any scripts you do not wish to be included in the export. This allows you to make backups of your bots and store them using a repository system of your choice.

When exporting you can either copy paste the code directly or select `download` to save the file as a JSON file.

## Import
This option allows you to import scripts in the JSON format created by the above export function. You can either import from a publically accessable url (for example your projects Github page) or manually paste the JSON.

_Warning_: Care should be excercised at this point as you will be overwritting any existing data. When you import, any script that will overwrite an existing one will be highlighted in red. You can skip importing a given script by unchecking scripts individually.

You may want to [create a new bot](https://botkit.groovehq.com/knowledge_base/topics/create-your-bot) before making any substantial changes in a production bot.




