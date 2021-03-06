# WelcomeBot

The welcome bot comments on the Github Issues and PRs of first time users at a organization level. It does so by keeping a running list of the github usernames of all users that have opened a PRs and Issues against a repo in your organization. Each list is stored in AWS DynamoDB so that no state is stored in the application itself.

## Configuration

### Credentials

WelcomeBot loads configuration via either a YAML config or environmental variables. The YAML config is a simple flat config named welcome_bot.yml. See the example file in this repo for a list of available configuration options. Each option can be configured via this YAML config or via an environmental variable of the same name (except capitalized). For example the YAML config contains the value 'aws_access_key_id', and the environmental variable 'AWS_ACCESS_KEY_ID' can also be used, and will take precedence over the YAML config value.

### Webhook Setup

WelcomeBot functions using either Github repo level webhook, or org level webhooks. At current the setup binary does not configure webhooks, which allows you to chose the hook configuration that works best for you. You can setup an organization hook via the organization setting page as follows:

![setup image](https://raw.githubusercontent.com/chef/welcome_bot/master/readme_images/setup.png)

### Message Configuration

WelcomeBot stores the messaging in DynamoDB in a table called WelcomeBot_Messages, which is created when running the setup command. Storing data in DynamoDB allows you to update the messages you want to send without having to restart the bot. These messages are per org so that WelcomeBot can handle webhooks from more than one repo.

![dynamo table image](https://raw.githubusercontent.com/chef/welcome_bot/master/readme_images/dynamo.png)

### Running as a Service

You probably want to run the bot under an init system that can handle logging and starting the service back up if it dies. Here's a sample systemd unit file to run the app via bundler

```
[Unit]
Description=WelcomeBot Github bot
After=network.target

[Service]
Type=simple
TimeoutSec=30
RestartSec=15s
WorkingDirectory=/home/ubuntu/welcome_bot/
ExecStart=/bin/bash -lc 'bundle exec bin/welcome_bot'
Restart=always

[Install]
WantedBy=multi-user.target
```

This assumes you're running Ubuntu, the repo is checked out `/home/ubuntu/welcome_bot/` and bundle install has already been performed in the repo directory.

## Contributing

For information on contributing to this project see <https://github.com/chef/chef/blob/master/CONTRIBUTING.md>

## License

- Author:: Tim Smith ([tsmith@chef.io](mailto:tsmith@chef.io))
- Copyright:: Copyright (c) 2016-2017 Chef Software, Inc.
- License:: Apache License, Version 2.0

```text
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
