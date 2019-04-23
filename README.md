# Post from R to Slack

When I schedule R scripts to run every day or week I like to keep track of them without having to dig through logs and files. This can be done using an app called `slackr` which allows the code to post to a Slack channel.

## First I have to set up Slack APIs 
This can be done by following the following steps:

- go to https://api.slack.com/slack-apps to set up the slack-API and webhook

![Build Slack App Site](build_slack_apps.jpg)

- Click on the green button `Create Slack App`

When the below window opens:

![Create App] (create_app.jpg)

- Chose the App Name & the Slack Workspace

- Click `Create App`

- go to `Basic Information` in the menu on the left

![Basic Information](basic_information.jpg)

- Click on `Incoming Webhooks`

- Move the slider by `Activate Incoming Webhooks` to `On`

![Incoming Webhooks](incoming_webhooks.jpg)

- Click on `Add New Webhook to Workspace`

- On the screen that comes up you should see a `Post to` section with a dropdown menu, chose there what channel you would like to post do (for demonstration purposes I chose `Slackbot, which is private to you`)

- Click the green button (`Authorize`), this will return you to the `Incoming Webhooks` screen where the `Webhook URL` section now has an entry starting with `https://hooks.slack.com/services/...` 

- Return to `Basic Information` and click on `Add features and functionality` and click on `Permissions`, this will return you to the `OAuth & Permissions` screen. The first section gives you the OAuth Access Token starting with `xoxp-...` which you **will need in your R code**.

![OAuth Tokens & Redirect URLs](oauth.jpg)

- Scroll down to `Scopes` and in `Select Permission Scopes` add type `chat:write:bot` into the `Add permission by scope or API method` dropdown menu.

- Click on `Save Changes`, which will prompt you to Reinstall the App, click on the link and get redirected to the same screen as before, again chose which channel you want to post to and `Authorize`.

- This will create a new entry starting with `https://hooks.slack.com/services/...` in the `Webhook URLs for Your Workspace` within `Incoming Webhooks` menu which you **will need for your R code**.

![Scopes](scopes.jpg)

**Viola, the Slack API is set up**

## Now to the R code
### .slackr
- Make a file called `.slackr` with the following entries
```
api_token: xoxp-....
channel: #slack_channel
username: slack_username
incoming_webhook_url: https://hooks.slack.com/services/...
```
### file_to_schedule.R
- Create the file that contains the code that will run regularly
- the following lines of code have to be added for the slack messaging to work
```
library(slackr)
slackr_setup(config_file = "/../.slackr")
text_slackr(paste0(Sys.time(), ' Text in Slack'), preformatted = TRUE)
```

**Viola, your code is posting to Slack**



