---
title: "Salesforce Continuous Integration with GitHub Actions"
date: 2019-08-24T22:12:46+09:00
image: "posts/img/20190825_3.png"
draft: false
---
After 6 months in the wait list, [GitHub Actions](https://github.com/features/actions) (v2) finally came to my account. It's a built-in CI/CD service for GitHub. I quickly tested it and I found it pretty good tool for GitHub users to start CI easily.

![](../img/20190825_1.png)

In this post, I'll show a simple example to realize continuous integration for Salesforce development. When a pull request is created, the following tasks will be automatically executed. 

* Create a scratch org
* Push source
* Run Apex tests
* Comment the org login url to the thread. (NOTICE: Don't do this in public repos)

## Initial setup
Create a connected app and a private key for JWT flow. Follow the steps in the [Trailhead project](https://trailhead.salesforce.com/en/content/learn/modules/sfdx_travis_ci/sfdx_travis_ci_connected_app).

## Save secrets
Store the consumer key of connected app as `SALESFORCE_CONSUMER_KEY` to repository secret. I also store secret key directly as `SALESFORCE_JWT_SECRET_KEY`. Or, you can store the decryption key and IV for encrypted secret key in the repository. And store the username of DevHub as `SALESFORCE_DEVHUB_USERNAME`.

![](../img/20190825_2.png)

## Define workflow
Define workflow in `.github/workflows/main.yaml`. For more information about workflow syntax, see [Workflow Syntax for GitHub Actions](https://help.github.com/en/articles/workflow-syntax-for-github-actions)
{{<gist shunkosa 905d131a96d9c8619ae039529c040b20>}}

### Highlights
* A workflow is made up of one ore more jobs. A job contains a sequence of tasks called steps. Jobs run in parallel by default and each job runs in a fresh instance. In the above workflow, for simplicity, single job named `build` runs.
* (line 9) : `checkout@master` runs `git fetch` and `git checkout $GITHUB_SHA`, so this action always checkouts the commit of the event trigger.
* (line 24) : The repository secrets are available in workflow by `${{ secrets.YOUR_SECRET_NAME }}`.
* (line 39) : GitHub API is used to comment to the pull request. You can use default `GITHUB_TOKEN` secret as access token. You don't have to generate a new token for CI/CD service!
* (line 40) : `github` default context is used to specify the pull request URL. For more information, see [Events that trigger workflows](https://help.github.com/en/articles/events-that-trigger-workflows).

## See how it works
![](../img/20190825_3.png)

Now you can easily and effectively review the pull request using the scratch org automatically created and tested. 

Next step is to delete the scratch org after the pull request is merged/closed. We need to store the org username somehow.

I like GitHub Actions from many views. Tightly-coupled with the repository (infinite possibilities to integrate with the repository!). Concurrency. Supporting multiple OS. Various 3rd party actions.

I hope more and more Salesforce developers try GitHub Actions. Share if you create an awesome action!
