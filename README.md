# gotify-cli-app - A simple Gotify cli push client

`gotify-cli-app` is a simple command line client that pushes notifications to the awesome [Gotify](https://github.com/gotify/server) REST-API. It works by setting default values for all required notification parameters, allowing you to override single parameters. This means you don't need to remember the syntax of `curl` or your server's URL and application tokens. You can even run `gotify-cli-app` without any parameters to send a predefined notification with default parameters.

## Installation

`gotify-cli-app` requires `python3` to run.

First install its dependencies by running:

```bash
pip3 install -r requirements.txt
```

Then copy and rename the `gotify-example.yaml` file to `gotify.yaml`. Edit the configuation file to suit your needs and place it in one of the locations described in the [configuration](#Default config file locations) section.

Install the script by copying it to a folder in your system's path, such as `/usr/local/bin`. You may also wish to rename the command in this step to for example if you want to have a shorter command to call.

```bash
cp gotify-cli-app /usr/local/bin/.
```

Make sure that the script is executable for all required users. For example, to make gotify-cli-app executable by all users:

```bash
chmod a+x /ust/local/bin/gotify-cli-app
```

## Configuration

The configuration file is a simple YAML file with the following structure:

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

### Default config file locations

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

### tokenMap

One feature of `gotify-cli-app` is the `tokenMap` defined in the configuration file. This file assigns human readable names (known as keys) to application tokens. This allows you to send notifications to different Gotify apps without having to remember their application tokens.

An entry of the `tokenMap` looks like this:

```yaml
ssh: '<ssh_app_token_here>
```

and can be used like so:

```bash
gotify-cli-app -k ssh -t "SSH login" -m "A user has logged in per SSH"
```

### Custom config file location

To load a config file from a custom directory the `-c/--config` argument may be used:

```bash
gotify-cli-app -c /path/to/config-file.yaml
```

### Running gotify-cli-app without a config file

To run `gotify-cli-app` withour a config file the following arguments must be provided:

```bash
gotify-cli-app -u https://notify.example.com -a <app_token_here>  -t "Hello" -m "Hello World" -p 10
```

## Usage

* Running gotify-cli-app with all default values read from a config file is as simple as running:

```bash
gotify-cli-app
```

* To list all possible arguments use the built-in help function:

```bash
gotify-cli-app --help
```

* To set a custom title and message:

```bash
gotify-cli-app -t "Hello" -m "Hello World"
```

* Pipe a message from stdin (notice the `-` following the `-m/--message` argument):

```bash
echo "Hello World" | gotify-cli-app -t "Hello" -m -
```

* Running gotify-app-cli with a different notification priority:

```bash
gotify-cli-app -t "Hello" -m "Hello World" -p 10
```

* To run gotify-cli-app with a different application token run:

```bash
gotify-cli-app -a <app_token_here> -t "Hello" -m "Hello World!
```

where `<app_token_here>` is the gotify app token.

* To use the `tokenMap` defined in the configuration file run:

```bash
gotify-cli-app -k <app_key_here> -t "Hello" -m "Hello World"
```

where `<app_key_here>` is the name of the application provided in the configuration file.