starter-python-bot
=============

## Overview
A simple starting point for creating a Beep Boop hostable, Python based Slack bot.

Visit [Beep Boop](https://beepboophq.com/docs/article/overview) to get the scoop on the the Beep Boop hosting platform. The Slack API documentation can be found [here](https://api.slack.com/).

## Assumptions
* You have already signed up with [Beep Boop](https://beepboophq.com) and have a local fork of this project.
* You have sufficient rights in your Slack team to configure a bot and generate/access a Slack API token.

## Usage

### Run locally
Install dependencies ([virtualenv](http://virtualenv.readthedocs.org/en/latest/) is recommended.)

	pip install -r requirements.txt
	export SLACK_TOKEN=<YOUR SLACK TOKEN>; python ./bot/app.py

Things are looking good if the console prints something like:

	Connected <your bot name> to <your slack team> team at https://<your slack team>.slack.com.

If you want change the logging level, prepend `export LOG_LEVEL=<your level>; ` to the `python ./bot/app.py` command.

### Run locally in Docker
	docker build -t starter-python-bot .
	docker run --rm -it -e SLACK_TOKEN=<YOUR SLACK API TOKEN> starter-python-bot

### Run in BeepBoop
If you have linked your local repo with the Beep Boop service (check [here](https://beepboophq.com/0_o/my-projects)), changes pushed to the remote master branch will automatically deploy.

### First Conversations
When you go through the `Add your App to Slack` flow, you'll setup a new Bot User and give them a handle (like @python-rtmbot).

Here is an example interaction dialog that works with this bot:
```
Joe Dev [3:29 PM]
hi @python-rtmbot

Slacks PythonBot BOT [3:29 PM]
Nice to meet you, @randall.barnhart!

Joe Dev [3:30 PM]
help @python-rtmbot

Slacks PythonBot BOT [3:30 PM]
I'm your friendly Slack bot written in Python.  I'll ​*​_respond_​*​ to the following commands:
>`hi @python-rtmbot` - I'll respond with a randomized greeting mentioning your user. :wave:
> `@python-rtmbot joke` - I'll tell you one of my finest jokes, with a typing pause for effect. :laughing:
> `@python-rtmbot attachment` - I'll demo a post with an attachment using the Web API. :paperclip:

Joe Dev [3:31 PM]
@python-rtmbot: joke

Slacks PythonBot BOT [3:31 PM]
Why did the python cross the road?

[3:31]
To eat the chicken on the other side! :laughing:
```

## Code Organization
If you want to add or change an event that the bot responds (e.g. when the bot is mentioned, when the bot joins a channel, when a user types a message, etc.), you can modify the `_handle_by_type` method in `event_handler.py`.

If you want to change the responses, then you can modify the `messenger.py` class, and make the corresponding invocation in `event_handler.py`.

The `slack_clients.py` module provides a facade of two different Slack API clients which can be enriched to access data from Slack that is needed by your Bot:

1. [slackclient](https://github.com/slackhq/python-slackclient) - Realtime Messaging (RTM) API to Slack via a websocket connection.
2. [slacker](https://github.com/os/slacker) - Web API to Slack via RESTful methods.

The `slack_bot.py` module implements and interface that is needed to run a multi-team bot using the Beep Boop Resource API client, by implementing an interface that includes `start()` and `stop()` methods and a function that spawns new instances of your bot: `spawn_bot`.  It is the main run loop of your bot instance that will listen to a particular Slack team's RTM events, and dispatch them to the `event_handler`.

## License

See the [LICENSE](LICENSE.md) file for license rights and limitations (MIT).
