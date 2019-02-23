<h1 align="center">Twitter, together!</h1>

<p align="center">
  <a href="https://travis-ci.com/gr2m/twitter-together" rel="nofollow"><img alt="Build Status" src="https://travis-ci.com/gr2m/twitter-together.svg?token=SMJUtZjXxPL3JRiMCqHx&branch=master"></a>
  <a href="https://greenkeeper.io/" rel="nofollow"><img src="https://badges.greenkeeper.io/gr2m/twitter-together.svg?token=fec4ee116d4210bb3f03e13bed6266d5fc8e8764def4f269753e522abfba3a19&ts=1550824957051"></a>
</p>

<p align="center">A GitHub action to tweet together using pull requests</p>

Shared twitter accounts are challenging. More often than not, nobody is using it out of fear to say something that would upset fellow maintainers.

`twitter-together` to the rescue!

This GitHub action uses files in order to send out tweets. You can utilize GitHub’s pull request reviews to create drafts and discuss before sending out a tweet. Even external people can contribute a tweet, just like they can contribute code.

<!-- toc -->

## Setup

1. Create a `.github/main.workflow` file with the content below (switch to the <kbd>`<> Edit new file`</kbd> tab to enter the code):

  ```workflow
  workflow "Tweet on push to main branch" {
    on = "push"
    resolves = ["Tweet"]
  }

  action "Tweet" {
    uses = "gr2m/twitter-together@master"
    secrets = ["GITHUB_TOKEN", "TWITTER_CONSUMER_KEY", "TWITTER_CONSUMER_SECRET", "TWITTER_ACCESS_TOKEN", "TWITTER_ACCESS_SECRET"]
  }

  # "push" event won’t work on forks, hence the 2nd workflow with "pull_request"

  workflow "Preview and validate tweets on pull requests" {
    on = "pull_request"
    resolves = ["Preview"]
  }

  action "Preview" {
    uses = "gr2m/twitter-together@master"
    secrets = ["GITHUB_TOKEN"]
  }
  ```

2. In order to get the `TWITTER_*` credentials, you need to create a twitter app with the account you want to tweet from. You can do that at https://apps.twitter.com/. The twitter app needs read and write permissions, all other configurations are irrelevant. Once you created the app, you find the credentials in the  <kbd>`Keys and tokens`</kbd>  tab. Enter the values for `TWITTER_CONSUMER_KEY`, `TWITTER_CONSUMER_SECRET`, `TWITTER_ACCESS_TOKEN` and `TWITTER_ACCESS_SECRET` in your repository’s secrets Settings.

3. After creating the `.github/main.workflow` or adding the workflows to your exiting ones, the `twitter-together` action will create a pull request which creates a `tweets/` folder with a `README.md` file in it. Merge it and follow its instructions to create your own tweet :)

## Contribute

All contributions welcome, [including tweets](tweets/), of course :) 

See [CONTRIBUTING.md](CONTRIBUTING.md)

## How it works

`twitter-together` is using two workflows

1. `push` event to publish new tweets
2. `pull_request` event to validate and preview new tweets

### The `push` event

When triggered by the `push` event, we first make sure that the current branch is the repository’s main branch. After that we load the changed files and look for `*.tweet` files in the `tweets/` folder or subfolders. If there are any, we send out a tweet for each new tweet file.

### The `pull_request` event

For the `pull_request` event, we only handle `opened` and `synchronize` actions. First we check if the pull request’s base branch is the repository’s main branch. After that we load the changed files and look for `*.tweet` files in the `tweets/` folder or subfolders. If there are any, we validate the length of each tweet. If one is to long, a failed check run with an explanation is created. If all tweets are valid, a check run with a preview of all tweets is created.

## Motivation

I think we can make Open Source more inclusive to people with more diverse interests by making it easier to contribute other things than code and documentation. For example, ever since working on [Hoodie](http://hood.ie/intro/), I thought that there is a big opportunity to accept editorial contributions in Open Source projects if there were only better tools for it. That’s when the idea for a collaborative twitter account using a repository as its backend came up, that was around 2014-2015. Finally with GitHub Actions, it became fairly straight forward to make it real.

I hope that you find the action useful and can’t wait to see for the first time when a project accepts a tweet from an external contributor. Please let me know if you use `twitter-together` in your project :)

## License

[MIT](LICENSE)
