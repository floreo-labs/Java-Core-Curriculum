# Twin Cities Cohort - Slack Bot Challenge 🤖
<br>
A [Slack bot](https://api.slack.com/bot-users) is a non-human "user" that interacts with the Slack messaging app. Bots might post messages to users or to channels, send reminders, look up information in response to a question or perform a calculation.  In this project, we'll work in teams to build bots that can preform simple interactions with the Dream Corp Tech Slack.

![slack bot example](https://api.slack.com/img/api/guide_bot_user.png)

Slack offers three different APIs that developers can use to interact with their service: the Web API, the Real Time Messaging API and the Events API . In this project, bots will only interact using the [Web API](https://api.slack.com/web). The Web API offers methods that can be used to list Dream Corps Tech channels, view message history on a given channel, and post and delete messages on the #bots channel.
<br>
## Setup
The following setup steps only need to be completed **once per team**:

1. Fork and clone [this repo](https://github.com/bbah93/Lab_Slackbot). Make sure both team members are added as contributors to the forked repo.

2. Go to https://dreamcorpstech.slack.com/apps/new/A0F7YS25R-bots. Make sure you are signed into the Dream Corps Tech Slack team. Choose a username for your bot and click the "Add bot integration" button.

3. Make a note of your **API token**. The token must always be kept in a **safe, secret** place. We will be storing our API keys in the `../SlackBot/api_token.txt` file, which has been added to our `.gitignore` file.

    > Always be careful when sharing API tokens! Be careful to never publish
> our bot user tokens in any public GitHub code repository.

4. Complete your bot's profile. Add an avatar image/emoji, a first and last name and a description of what your bot does. When you are finished, click the "Save Integration" button.

5. **Both team members:** join the #bots channel on Slack so you can see what your bot is up to. 

## Requirements
1. Form a clear picture of the project requirements. Spend some time reading through:
    - This readme doc.
    - The [Slack Web API docs](https://api.slack.com/web) and [Basic message formatting guidelines](https://api.slack.com/docs/message-formatting).
    - The code that has been provided in the `Lab_SlackBot` project -- particulary the Slack.java class, which provides methods that your bot can use to interact with Slack's Web API.
    - Use the IntelliJ TODO panel (⌘6) to navigate to each of the project TODOs.

2. For both of the following Slack API JSON objects, write a Java class to parse and represent it as a Java object. Each team member should be responsible for parsing at least one of the classes:
    - [user](https://api.slack.com/types/user) -> User.java
    - [attachment](https://api.slack.com/docs/message-attachments) -> Attachment.java

    Attachment.java and User.java files are already provided in the `model` subpackage -- just complete the TODOs as marked. See the Channel.java class for an example of how your completed classes should look.

3. In Bot.java, design and implement your bot code! It's up to your team to decide what your bot does, but at a minimum, when your program runs it should post a message with some content to the #bots channel. See below for some ideas.

4. (Optional / Bonus!) In `Slack.java`, implement the `sendMessageWithAttachments(String messageText, List<Attachment> attachments)`, which should take in a `String messageText` and a `List<Attachment> attachments` to post to the #bots channel. It should return a `SendMessageResponse`. If your bot will send messages with attachments, you must implement this method. Read https://api.slack.com/docs/message-attachments to familiarize yourself with how attachment URL parameters should be formatted.

5. Respect the **bot etiquette guidelines**:
    - Bots may only post messages to the #bots channel! No direct messages or posting to pre-existing channels like #general or #random.
    - No spamming or flooding the channel with messages: bots should post at most one or two messages at a time.
    - Keep it classroom friendly :)

## Some bot ideas
- Pig Latin Bot: looks at the text of the last message that was posted to the channel and re-posts it in [pig latin](https://en.wikipedia.org/wiki/Pig_Latin) (or rot13!).
- Random Fact Bot: post a random fact or programming tip from a curated list.
- Inspirational Image Bot: post a random image link from a selected list of favorite image URLs.
- Mashup Bot: get and post interesting content from [another API](https://gist.github.com/afeld/4952991).
- Can you implement a bot that uses [message buttons](https://api.slack.com/docs/message-buttons)?
- Be creative! The sky is the limit 🌈

## Take 15 minutes to discuss the following questions:
- What will your bot do? What messages will it post? Be specific!
- Draw out a rough blueprint for how you will achieve this programmatically -- it's ok if you don't know all the specifics yet, just sketch out some ideas for how your program might flow. Think about: If your bot is posting data (i.e. images, quotes, facts), where is the data coming from? How is your bot interacting with and responding to other messages / users on the channel? For example:
>Cat Bot: The program will have an ArrayList of URLs of cat images that we find on the internet. When it starts, the program will check the last 100 messages on the #bots channel using the `listMessages()` method. If there is a message that contains the word "cat" in its text, the program with post a message with the text "CAT!!!" and a link to one of our cat image URLs.

- How will your team divide tasks?
- How will your team coordinate and collaborate throughout the week?

## Resources
- [Slack Web API docs](https://api.slack.com/web)
- [JSON Parsing lesson plan](https://github.com/floreo-labs/Java-Core-Curriculum/tree/master/lessons/json)

## Submission

1. Each team member create a new branch in your forked Lab_SlackBot repo named after you to identify their work. Each commmit should be short but descriptive enough to hint at the work you've done. When you finish each feature, add, commit and push your changes. On GitHub, open a new Pull Request and assign it to your teammate to review and merge.

2. This assignment is due by ***5pm on Friday 10/23***. All code should be committed and pushed by the deadline. Each team member should complete the Google Form (check Slack closer to submission date!) to submit.
