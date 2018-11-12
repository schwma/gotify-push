# gotify-cli-app - A simple Gotify cli push client

`gotify-cli-app` is a simple command line client that pushes notifications to awesome [Gotify](https://github.com/gotify/server) REST-API. It works by setting default values for all required notification parameters, allowing you to override single parameters. This means you don't need to remember the syntax of `curl` or your servers URL and application tokens. You can even run `gotify-cli-app` without any parameters to send a predifined notification.

## Installation

gotify-cli-app requires `python3` to run.

First install its dependencies by running:

```bash
pip3 install -r requirements.txt
```

Then copy and rename the `gotify-example.yaml` file to `gotify.yaml`. Edit the configuation file to suit your need and place it in one of the locations described in the section Configuration.

Install the script by copying it to a folder in your system's path, such as `/usr/local/bin`. You may also wish to rename the command in this step to suit your needs.

```bash
cp gotify-cli-app /usr/local/bin/.
```

Make sure that the script is executable for all required users. For example, to make gotify-cli-app executable by all users:

```bash
chmod a+x /ust/local/bin/gotify-cli-app
```

## Configuration

The configuration file is a simple YAML file in the following format:

```yaml
url: 'https://notify.example.com'
default:
  token: '<app_token_here>'
  title: 'Server'
  message: 'Hey, listen!'
  priority: 10
tokenMap:
  cron: '<cron_app_token_here>'
  ssh: '<ssh_app_token_here>'
```

A configuration file needs to be placed in one of the following locations:

```bash
./gotify.yaml
./gotify.yml
~/.gotify.yaml
~/.gotify.yml
~/.gotify-cli-app/gotify.yaml
~/.gotify-cli-app/gotify.yml
~/.config/gotify-cli-app/gotify.yaml
~/.config/gotify-cli-app/gotify.yml
/etc/gotify-cli-app/gotify.yaml
/etc/gotify-cli-app/gotify.yml
```

Entries that are higher up in this list will overwrite entries below it if multiple configuration files exist.

One feature of gotify-cli-app is the `tokenMap` defined in the configuration file. This file assigns human readable names (known as keys) to application tokens. This allows you to send notifications to different Gotify applications without having to remember their application tokens.

An entry of the tokenMap looks like this:

```yaml
ssh: '<ssh_app_token_here>
```

and can be used like so:

```bash
gotify-cli-app -k ssh -t "SSH login" -m "A user has logged in per SSH"
```

## Usage

Running gotify-cli-app with all default values is as simple as running:

```bash
gotify-cli-app
```

To set a custom title and message:

```bash
gotify-cli-app -t "Hello" -m "Hello World"
```

Pipe message from stdin:

```bash
echo "Hello World" | gotify-cli-app -t "Hello" -m -
```

Running gotify-app-cli with a different notification priority:

```bash
gotify-cli-app -t "Hello" -m "Hello World" -p 10
```

To run gotify-cli-app with a different application token run:

```bash
gotify-cli-app -a <app_token_here> -t "Hello" -m "Hello World!
```

where `<app_token_here>` is the gotify app token.

To use the `tokenMap` defined in the configuration file run:

```bash
gotify-cli-app -k <app_key_here> -t "Hello" -m "Hello World"
```

where `<app_key_here>` is the name of the application provided in the configuration file.

To list all possible arguments use the built-in help function:

```bash
gotify-cli-app --help
```