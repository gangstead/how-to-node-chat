class: middle
background-image: url(slides/hubot-dark.png)

# How to Node Chat
### Steven Gangstead

@Gangstead

.round[![:scale 16%](slides/vinli-logo.gif)]
.center[.footnote[slides - https://gangstead.github.io/how-to-node-chat]]
---

# How To Chat Bot

- Hubot Overview
- Generate Your Own
- Connect to Slack
- Deploy to Heroku
- Connect to Gitter (if time)
- Links for further reading
---
class: center, middle
# Hubot Overview

---
# Hubot Overview

- Github, Inc internal project (on [Campfire](https://campfirenow.com/))
- Open sourced in [2011](https://github.com/blog/968-say-hello-to-hubot)
- Extensible to any chat client
- Easy to deploy
---
# Key Technologies
- .round[![:scale 16%](slides/hubot-small.png)] .round[![:scale 30%](slides/nodejs.jpg)] .round[![:scale 15%](slides/coffeescript.png)]
- ![:scale 30%](slides/npm-logo.png) ![:scale 20%](slides/yeoman-logo.png)
- ![:scale 30%](slides/Heroku_logo.png) .round[![:scale 9%](slides/slack.png)]  .round[![:scale 30%](slides/gitter.png)] + [28 others](https://hubot.github.com/docs/adapters/)
---
# What it does?
## "At his core, Hubot simply idles in various rooms and waits for specific strings to go by in the chat."

???
- Quote from 2011 blog post
- https://github.com/blog/968-say-hello-to-hubot
- We can do better in terms of gender.  Can robots even have gender?
---
# What it does?
## "At ~~his~~ its core, Hubot simply idles in various rooms and waits for ~~specific~~ matching strings to go by in the chat."
```coffeescript
robot.hear /problem\??/i, (msg) ->
  msg.send "http://cl.ly/BG7R/trollface.jpg"
```
.center[.round[![:scale 15%](http://f.cl.ly/items/0z050y2v0W1f2S3b2o39/trollface.jpg)]]
---
# Anatomy - Top Level
- `bin/hubot` - shell script to start hubot
 - Reads environment variables or command line arguments
- `index.coffee` - Main entry point
 - Exposes main loadBot function and all other top level modules
???
Command line arguments take precedence over env vars
---
# Anatomy - Next Level
- `User` - Represents a participating user in the chat
- `Brain` - Storage (key-value and Users) with hooks to make it persistent
- `Robot` - Robots receive messages from a chat source and dispatch them to matching listeners
- `Adapter` - A specific interface to a chat source for robots
- `Response` - Responses are sent to matching listeners
- `Listener` - Listeners receive every message from the chat source and decide if they want to act on it
- `Message` - Represents an incoming message

???
- These descriptions are taken from the source code
- Robot - I think you end up with one robot per chat source and your modules add lots of listeners to it
---
# Anatomy - TL;DR
- Source code not too terribly long
- Consists of framework and lots of hooks to extend it
 - It's easy to make it short: whenever they get to something that would be hard just leave a hook
---
class: center, middle
# Hubot Generator
```js
npm install -g yo generator-hubot
```
---
```sh
yo hubot
                     _____________________________
                    /                             \
   //\              |      Extracting input for    |
  ////\    _____    |   self-replication process   |
 //////\  /_____\   \                             /
 ======= |[^_/\_]|   /----------------------------
  |   | _|___@@__|__
  +===+/  ///     \_\
   | |_\ /// HUBOT/\\
   |___/\//      /  \\
         \      /   +---+
          \____/    |   |
           | //|    +===+
            \//      |xx|
```
???
If doing this interactively
- go to directory
- `mkdir mybot`
- `cl mybot`
- `yo hubot`
- `atom .`
---
```sh
                    _____________________________
_____              /                             \
\    \             |   Self-replication process   |
|    |    _____    |          complete...         |
|__\\|   /_____\   \     Good luck with that.    /
  |//+  |[^_/\_]|   /----------------------------
 |   | _|___@@__|__
 +===+/  ///     \_\
  | |_\ /// HUBOT/\\
  |___/\//      /  \\
        \      /   +---+
         \____/    |   |
          | //|    +===+
           \//      |xx|

```
---
# What did yeoman do?
- `bin/hubot` script
- `package.json` dependencies
- `external-scripts.json`
---
```sh
exec node_modules/.bin/hubot --name "mybot" "$@"
```
???
"$@" is just an array of current arguments, passing them on to main hubot script
---
```json
"dependencies": {
  "hubot": "^2.16.0",
  "hubot-diagnostics": "0.0.1",
  "hubot-google-images": "^0.2.1",
  "hubot-google-translate": "^0.2.0",
  "hubot-help": "^0.1.1",
  "hubot-heroku-keepalive": "^1.0.0",
  "hubot-maps": "0.0.2",
  "hubot-pugme": "^0.1.0",
  "hubot-redis-brain": "0.0.3",
  "hubot-rules": "^0.1.1",
  "hubot-scripts": "^2.16.2",
  "hubot-shipit": "^0.2.0",
  "hubot-slack": "^3.3.0"
}
```
---
```json
[
  "hubot-diagnostics",
  "hubot-help",
  "hubot-heroku-keepalive",
  "hubot-google-images",
  "hubot-google-translate",
  "hubot-pugme",
  "hubot-maps",
  "hubot-redis-brain",
  "hubot-rules",
  "hubot-shipit"
]
```
---
# What didn't yeoman do?
- Write any code
---
# Running locally
- `./bin/hubot`
- `hubot help`
- `ship it`
---
class: center, middle
# Connect to Slack

---
# Connect to Slack
![:scale 40%](slides/slack-configure-integrations.png) ![:scale 55%](slides/slack-configure-integrations-2.png)

---
# Connect to Slack
`HUBOT_SLACK_TOKEN=blahblahblah-copied-from-slack bin/hubot --adapter slack`
```sh
INFO Connecting...
INFO Logged in as shufflebot of gangstead, but not yet connected
INFO Slack client now connected
ERROR hubot-heroku-alive included, but missing HUBOT_HEROKU_KEEPALIVE_URL. `heroku config:set HUBOT_HEROKU_KEEPALIVE_URL=$(heroku apps:info -s  | grep web_url | cut -d= -f2)`
INFO hubot-redis-brain: Using default redis on localhost:6379
```
---
class: center, middle
# Deploy to Heroku

---
# Deploy to Heroku
### Install [Heroku Toolbelt](https://toolbelt.heroku.com/)
```sh
heroku create shuffle-bot
heroku config:add HUBOT_SLACK_TOKEN=xoxo-1234567890-BLAHBLAH1234567890
heroku config:add HEROKU_URL=http://shuffle-bot.herokuapp.com
git add --all
git commit -m "arise shuffle-bot"
git push heroku master
```
???
Heroku name must be unique.  Don't do this in a git repository
---
# Deploy to Heroku bonus
- `heroku addons:add redistogo:nano`
---
class: center, middle
# Connect to Gitter

---
# Connect to Gitter
- Get your "Personal Access" token from https://developer.gitter.im/apps
- `npm install --save hubot-gitter2`
- `HUBOT_GITTER2_TOKEN=<your token> ./bin/hubot -a gitter2`
- Bot now talks under your account
???
This one is buggier than the slack one.
---
class: center, middle
# Why?
---
# Why?
## Serious Things
- Github pull requests: notify and manage
- Continuous Integration: warn about failures
- [Deploying code](http://githubengineering.com/deploying-branches-to-github-com/)
 - Easier on boarding: just go look at the build channel
 - High visibility because history in the same application team is already using and searching
 - Actions mixed with discussion
- [Stand up alarm](https://github.com/hubot-scripts/hubot-standup-alarm)
???
Source for the benefits (in addition to the githubengineering post): http://www.ianbicking.org/blog/2014/02/hubot-chat-web-working-in-the-open.html
---
# Why?
## Useful / Fun Things
- Weather / traffic
- The [line at Shake Shak](https://github.com/github/hubot-scripts/blob/master/src/scripts/shakeshack.coffee)
- pug bomb
- business cat (responds to jargon)![:scale 25%](http://i.imgur.com/8WsH50B.jpg)
---
# Extensions
## External
```sh
npm search hubot-scripts <search term>
```
![search](slides/npm-hubot-search.png)
## Bundled
- http://hubot-script-catalog.herokuapp.com/recent
???
Or, you know, google.  CLI search seems to work better than npmjs.org search because it searches the keywords in the package.json, the website appears to be name only.

External - Add to package.json and external-scripts.json, `npm install`)
Bundled -  Copy to `\scripts`
---
# Coffeescript not mandatory
`\scripts` scanned for `.coffee` and `.js` files
```coffeescript
pongs = ["pong", "pool"]
module.exports = (robot) ->
  robot.hear /\b(ping)\b/i, (msg) ->
    msg.send msg.random pongs
```
```js
var pongs = ["pong", "pool"]
module.exports = function(robot) {
  return robot.hear(/\b(ping)\b/i, function(msg) {
    return msg.send(msg.random(pongs));
  });
};
```
---
class: center, middle
# Extension Idea: D&D Dungeon Master
## Send PR to: https://github.com/samiconductor

---
# Explore further - links

- Main page: https://hubot.github.com/
- https://github.com/blog/968-say-hello-to-hubot
- https://github.com/blog/1241-deploying-at-github
- http://githubengineering.com/deploying-branches-to-github-com/
- Podcast: [On Culture and Remoteness at GitHub with Paul Betts and Justin Spahr-Summers](http://hanselminutes.com/375/on-culture-and-remoteness-at-github-with-paul-betts-and-justin-spahr-summers)
- http://www.ianbicking.org/blog/2014/02/hubot-chat-web-working-in-the-open.html
- http://blog.npmjs.org/post/128237577345/how-to-build-a-slackbot-deploy-an-app-to-heroku
- https://www.npmjs.com/browse/keyword/hubot-scripts
---

# The End

- Shameless Plug for NodeSchool Dallas
 - Next meetup 9/23 (tomorrow!)
 - Scopes, Chains and Closures
 - http://www.meetup.com/Nodeschool-Dallas/events/219510546/

![:scale 20%](slides/nodeschool-dallas-skyline.png)
