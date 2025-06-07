---
date: 2025-05-19T22:01:37+02:00
# description: ""
# image: ""
lastmod: 2025-05-19
showTableOfContents: true
# tags: ["",]
title: "Automate countdown messages on Signal using Go and signal-cli"
type: "post"
---

[Signal](https://signal.org/) is an open-source, encrypted instant messaging service developed by the non-profit [Signal Foundation](https://signalfoundation.org/) and its subsidiary, Signal Messenger LLC. It is funded through grants and donations.

I use Signal with people who prefer an easy-to-use yet secure alternative to WhatsApp or Telegram, and I want to schedule reminder messages for some of them.

While Signal doesn’t offer an official API, a community-maintained command-line interface, [signal-cli](https://github.com/AsamK/signal-cli), provides a way to interact with the service.

## Installation

For the most up-to-date installation instructions and alternative methods, refer to the [installation section](https://github.com/AsamK/signal-cli?tab=readme-ov-file#installation) of the project’s README.
Here, I’ll walk through the installation using a community-maintained APT repository.

On Debian/Ubuntu based systems run the following commands:

```bash
sudo curl -sL -o /etc/apt/trusted.gpg.d/morph027-signal-cli.asc https://packaging.gitlab.io/signal-cli/gpg.key ## Add repo signing key to apt
echo "deb https://packaging.gitlab.io/signal-cli signalcli main" | sudo tee /etc/apt/sources.list.d/morph027-signal-cli.list # Add repo to apt
sudo apt-get install signal-cli-native # Install signal-cli
sudo apt-get install signal-cli-dbus-service # Install DBus service
```

## Account setup

Signal uses mobile telephone numbers to register and manage user accounts.
Register a phone number with SMS verification with the `register` command.
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

## Sending messages

## Scheduling messages with cron
