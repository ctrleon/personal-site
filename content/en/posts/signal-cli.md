---
date: 2025-05-19T22:01:37+02:00
# description: ""
# image: ""
lastmod: 2025-05-19
showTableOfContents: false
# tags: ["",]
title: "Scheduling Signal messages using signal-cli and cron"
type: "post"
---

I prefer using Signal for my private chats because it's open source.
Signal does not provide a direct API but there is a command line interface [signal-cli](https://github.com/AsamK/signal-cli) available.

## Installation

You can build signal-cli, use the provided binary files, the Docker image or a community-maintained Linux package if available for your distribution of choice.

I prefer using the Linux packages to manage all of my packages on my Debian 12 server.
On Debian/Ubuntu based systems run the following commands:

```bash
sudo curl -sL -o /etc/apt/trusted.gpg.d/morph027-signal-cli.asc https://packaging.gitlab.io/signal-cli/gpg.key ## Add repo signing key to apt
echo "deb https://packaging.gitlab.io/signal-cli signalcli main" | sudo tee /etc/apt/sources.list.d/morph027-signal-cli.list # Add repo to apt
sudo apt-get install signal-cli-native # Install signal-cli
sudo apt-get install signal-cli-dbus-service # Install DBus service
```

## Account activation

Register a phone number with SMS verification with the following command.
Account is the phone number in internatinal format you want to register.

```bash
signal-cli -a ACCOUNT register
```

If SMS is not available, you can attempt requesting a voice call verification with the command:

```bash
signal-cli -a ACCOUNT register --voice
```

The standard output from the `register` command might instruct you to generate a captcha token.
When you have the captcha code, execute the `register` command again with the `--captcha` argument.

```bash
signal-cli -a ACCOUNT register --captcha "signalcaptcha..."
```

If you get an error try to get the captcha again.
Else you can proceed with the verification with the `verify`command.
If your Signal account has a PIN code, add the argument `--pin PIN_CODE`.
Else you might have to wait some hours until it resets to try again.

```bash
signal-cli -a ACCOUNT verify CODE 
```
