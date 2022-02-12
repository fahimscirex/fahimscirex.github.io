---
title: "Host Telegram Userbot on GitHub w/o Any Charge"
date: 2021-10-15T19:17:55+06:00
draft: false
aliases: '/post/journal/deploy-userbot-with-github-action/'
categories:
- Techtalk
tags:
- Userbot
- Telegram
- GitHub
description: Most of the Cloud Platform bans account for hosting userbots. Well, good news for you! You don't have to worry about getting banned again. Here's how.
---

Most of the Cloud Platform doesn't allow hosting userbot. Your account could get banned if you try to host any or if you're lucky you'll be able to use your bot for a while. Setting up a new account frequently and hosting bot on it is a hell of a work, that's why it's wise to use GitHub Actions.

GitHub Actions' Workflows aren't meant to be used as a Cloud Platform (eg. Heroku, Railway) but if it can get your job done then why won't you take the advantage? Moreover, you won't have to pay a single penny.

Before taking you to the procedure there's one thing you should know. **Your bot will restart in every 6 hours**. Cause maximum Action timeout is 360 minutes. Well, Heroku apps also restarts in every 6 hours but the main difference here is, your data won't be stored on GitHub. 

For example if you use ytdl to download a video usually it goes into download folder and stays there unless you delete it. But with GitHub Action those downloaded files will be gone when your Actions time is finished. I hope you got me.

I'll share Action script with support for [Ultroid](https://ultroid.tech) and [TG-Userbot](https://github.com/TG-UserBot/TG-UserBot). 

## Requirements
- GitHub Account
- One public repo
- One private repo

## Fork Public Repo
Go to https://github.com/fahimscirex/userbot-workflow and fork the repo.
You're going to deploy your bot in this repository.
Change the contents of `.github/workflows/deploy.yml` as per your needs. If you want to use Ultroid then keep `ultroid.sh` or else replace with `tguserbot.sh`.

## Create a Private Repo
Forked repo can't be switched to private, so you have to create this on your own. This repo is supposed to contain
- `.env` (for Ultroid)
or
- `config.ini` (for TG-Userbot)

Why private? Cause you'll keep your personal data here. You would never like to share your APIs or Sessions with others, right? BTW, I'm not going write about what will be in those files. Check their respective documentation to learn more. But I can give a suggestion, you should use Railway to host a Redis server.

## Add Secrets in Public Repo
As your forked repo is going to stay public or you won't get unlimited Action time, you've to add some secrets. So that you can access the data of your private repo. Go to *https://{your_forked_repo}/settings/secrets/actions* to add secrets.

You've to add these following secrets -

- `GH_NAME` : Your GitHub username.
- `GH_MAIL` : Mail that you use to sign into GitHub.
- `CREDS` : Link to your private repo but without (https://) eg. github.com/{username}/{private repo}.
- `GH_TOKEN` : Go to https://github.com/settings/tokens to generate a token. Tick repo, workflow and user and hit generate. Copy the token and add it to the secrets.
- `GH_REPO` : Your current repo. `username/reponame` e.g. fahimscirex/userbot-workflow
- `TZ` : Timezone. Findout your timezone and put it in your secret. e.g. Asia/Dhaka.

## Deploy Your Bot
Our work is done. Now run a workflow and check the logs. If everything goes well your bot will restart in every six hours. You can check loop.txt to know when your bot restarted.
