# Tlapbot
Tlapbot is an [Owncast](https://owncast.online/) bot, aiming to add the feature of channel points and
channel point redeems to Owncast.

This bot is currently in-development. The goal is to have a feature set on par
with Twitch channel points, while making use of Owncast webhooks and especially
[External actions](https://owncast.online/thirdparty/actions/).
## Features
Currently, the bot gives points to everyone in chat -- the interval can be
configured in the config, as well as the amount of points given.

The users in chat can then use their points on redeems. The bot currently
only has one hardcoded redeem, but I'd like to make this configurable,
so that every Owncast streamer can set up their own redeems that best fit
their stream.

The redeems then show on a "Redeems dashboard" that everyone can view
at the flask server's URL, which can be included in Owncast
as an External action, a single button that displays information about
recent redeems.
## Setup
The Python prerequisites for running tlapbot are the libraries `flask`,
`requests` and `apscheduler`.
### First time setup
Install prerequisites:
```bash
pip install flask
pip install requests
pip install apscheduler
```
(Or install them in your virtual environment if you prefer to use one)

Initialize db:
```bash
python -m flask init-db
```
Create a `tlapbot/config.py` file and fill it in as needed.
Default values are included in `tlapbot/default_config`, and values in
`config.py` overwrite them.

Tlapbot will probably not work if you don't overwrite these:
```bash
SECRET_KEY # get one from running `python -c 'import secrets; print(secrets.token_hex())'`
OWNCAST_ACCESS_TOKEN # get one from owncast instance
OWNCAST_INSTANCE_URL
```
### Owncast setup
In Owncast, navigate to the admin interface at `/admin`,
and then go to Integrations.
#### Access Token
In Access Tokens, generate an Access Token to put in
`tlapbot/config.py`. At the moment, the only permission the Access Token needs
is sending messages, the bot doesn't perform any administrative actions.
#### Webhook
In webhooks, create a Webhook, and point it at your bot's URL with
`/owncastWebhook` added.

In debug, this will be something like `localhost:5000/owncastWebhook`,
or, if you're not running the debug Owncast instance and bot on the same machine,
you can use a tool like [ngrok](https://ngrok.com/)
to redirect the Owncast traffic to your `localhost`.
#### External Action
In External Actions, point the external action to your bot's URL with `/dashboard` added.

In debug, this might be something like `localhost:5000/dashboard`,
or you can use a tool like ngrok again.

**Example:**
```
URL: MyTlapbotServer.com/dashboard
Action Title: Redeems Dashboard
```

### Running in debug:
Set the FLASK_APP variable:
```bash
export FLASK_APP=tlapbot
```
or in Powershell on Windows:
```powershell
$Env:FLASK_APP = "tlapbot"
```
Run the app (in debug mode):
```bash
python -m flask --debug run 
```
### Running in prod:
To be added when I actually run a prod version of the bot.
## Config
Values you can include in `tlapbot/config.py` to change how the bot behaves.
### Channel points interval and amount
`POINTS_CYCLE_TIME` decides how often channel points are given to users in chat,
in seconds. 

`POINTS_AMOUNT_GIVEN` decides how many channel points users receive.

By default, everyone receives 10 points every 600 seconds (10 minutes).
