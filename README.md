![gocd-slack-server: A ruby library for managing GitHub pull requests](https://raw.githubusercontent.com/sotownsend/gocd-slack-server/master/logo.png)

[![Gem Version](https://badge.fury.io/rb/gocd-slack-server.svg)](http://badge.fury.io/rb/gocd-slack-server)
[![Build Status](https://travis-ci.org/sotownsend/gocd-slack-server.svg?branch=master)](https://travis-ci.org/sotownsend/gocd-slack-server)
[![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/sotownsend/gocd-slack-server/trend.png)](https://bitdeli.com/free "Bitdeli Badge")
[![License](http://img.shields.io/badge/license-MIT-green.svg?style=flat)](https://github.com/sotownsend/gocd-slack-server/blob/master/LICENSE)

# What is this?
gocd-slack-server is a standalone server that relays gocd information directly to slack.  *it is not a gocd plugin and relies on gocd's API for
communication*

# Installation
```sh
#Setup
sudo gem install gocd-slack-server

#Run using a service hook with the name 'bot_name'
gocd-slack-server "https://hooks.slack.com/services/..." "bot_name"
```

#Create a new gocd-slack-server object, each gocd-slack-server object targets (1) repository.
gocd-slack-server = gocd-slack-server.new(user:"github_username", pass:"github_password", repo:"my_repository")

#Get a list of all open pull requests (An array of pull numbers)
open = gocd-slack-server.pull_requests

#Get a list of all open pull requests marked as pending or success
open_and_pending_or_success = gocd-slack-server.open_or_pending_pull_requests

#Create a new pull request to merge 'my_branch' into 'master' with the title 'My pull request' and the message 'Hey XXX...'
pull_number = gocd-slack-server.create_pull_request(from:"my_branch", to:"master", subject:"My pull request", message:"Hey XXXX, can you merge this for me?")

#Comment on that pull request
gocd-slack-server.write_comment_to_pull_request(pull_number, "Test Comment")

#Get all comments
comments = gocd-slack-server.comments_for_pull_request(pull_number)

#Get the SHA of the 'from' branch of a certain pull request
gocd-slack-server.sha_for_pull_request(pull_number)

#Set the status of a pull request to pending (Other options include 'error', 'failed', and 'success')
gocd-slack-server.set_pull_request_status(pull_number, "pending")

#Set the status of a pull request to ready
gocd-slack-server.set_pull_request_status(pull_number, "success")

#Merge the request (Will NOT use GitHub's pull request merge, will merge commits into history as-is)
gocd-slack-server.merge_pull_request(pull_number)
```

# Organization Repositories
If your repositories are not owner by you, i.e. they are owned by an organization or another user who has granted you permissions, you will need to
pass the `owner` field for the other individual or organization.

```ruby
#Create a new gocd-slack-server object, each gocd-slack-server object targets (1) repository in an organization.
gocd-slack-server = gocd-slack-server.new(user:"github_username", pass:"github_password", repo:"my_repository", owner:"my_organization")

```

## Requirements

- Ruby 2.1 or Higher

## Communication

- If you **found a bug**, submit a pull request.
- If you **have a feature request**, submit a pull request.
- If you **want to contribute**, submit a pull request.

## Installation

Run `sudo gem install gocd-slack-server`

## Known issues

1. GitHub does not register commits immediately after a push is received. Things like `sha_for_pull_request` will return old values if you don't wait
   several seconds
2. GitHub's status API for pull requests returns 'pending' even if the UI says 'success'. We account for this bug, but if it is fixed in the future,
   then our specs will catch it

---

## FAQ

### When should I use gocd-slack-server?

When you want to automate GitHub pull requests.  gocd-slack-server provides the necessary facilities for you to authenticate and control GitHub pull requests in
any way you wish.  Duplicate the functionality of many popular CI solutions.

### What's Fittr?

Fittr is a SaaS company that focuses on providing personalized workouts and health information to individuals and corporations through phenomenal interfaces and algorithmic data-collection and processing.

* * *

### Creator

- [Seo Townsend](http://github.com/sotownsend) ([@seotownsend](https://twitter.com/seotownsend))

## License

gocd-slack-server is released under the MIT license. See LICENSE for details.
