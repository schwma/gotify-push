# gotify-push - A simple CLI client for the Gotify REST-API

`gotify-push` is a simple command line client, written in python, that pushes notifications to the awesome [Gotify](https://github.com/gotify/server) REST-API. It works by setting default values for all required notification parameters, allowing you to override single parameters. This means you don't need to remember the syntax of `curl` or your server's URL and application tokens. You can even run `gotify-push` without any parameters to send a predefined notification with default parameters.

## Installation

`gotify-push` requires `python3` to run.

First install its dependencies by running:

```bash
pip3 install -r requirements.txt
```

Then copy and rename the `gotify-example.yaml` file to `gotify.yaml`. Edit the configuation file to suit your needs and place it in one of the locations described in the [configuration](#default-config-file-locations) section.

Install the script by copying it to a folder in your system's path, such as `/usr/local/bin`. Feel free to rename the script in this step if you wish to do so.

```bash
cp gotify-push /usr/local/bin/.
```

Make sure that the script is executable for all required users. For example, to make `gotify-push` executable by all users:

```bash
chmod a+x /usr/local/bin/gotify-push
```

## Configuration

The configuration file is a simple YAML file with the following structure:

```yaml
# Url of the Gotify server (including the protocol to use)
url: 'https://notify.example.com'
# Default notification parameters
default:
  token: '<app_token_here>'
  title: 'Server'
  message: 'Hey! Listen!'
  priority: 10
# Token map to map keys to tokens
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
~/.gotify-push/gotify.yaml
~/.gotify-push/gotify.yml
~/.config/gotify-push/gotify.yaml
~/.config/gotify-push/gotify.yml
/etc/gotify-push/gotify.yaml
/etc/gotify-push/gotify.yml
```

Entries that are higher up in this list will overwrite entries below it if multiple configuration files exist.

### tokenMap

One feature of `gotify-push` is the `tokenMap` defined in the configuration file. This file assigns human readable names (known as `key`s in `gotify-push`) to application tokens. This allows you to send notifications to different Gotify apps without having to remember their application tokens.

An entry of the `tokenMap` looks like this:

```yaml
ssh: '<ssh_app_token_here>
```

and can be used like so:

```bash
gotify-push -k ssh -t "SSH login" -m "A user has logged in per SSH"
```

### Custom config file location

To load a config file from a custom directory the `-c/--config` argument may be used:

```bash
gotify-push -c /path/to/config-file.yaml
```

### Running gotify-push without a config file

To run `gotify-push` without a config file the following arguments must be provided:

```bash
gotify-push -u https://notify.example.com -a <app_token_here>  -t "Hello" -m "Hello World" -p 10
```

## Usage

* Running `gotify-push` with all default values read from a config file is as simple as running:

```bash
gotify-push
```

* To list all possible arguments use the built-in help function:

```bash
gotify-push --help
```

* To set a custom title and message:

```bash
gotify-push -t "Hello" -m "Hello World"
```

* Pipe a message from `stdin` (notice the `-` following the `-m/--message` argument):

```bash
echo "Hello World" | gotify-push -t "Hello" -m -
```

* Running `gotify-push` with a different notification priority:

```bash
gotify-push -t "Hello" -m "Hello World" -p 10
```

* To run `gotify-push` with a different application token run:

```bash
gotify-push -a <app_token_here> -t "Hello" -m "Hello World"
```

where `<app_token_here>` is the gotify app token.

* To use the `tokenMap` defined in the configuration file run:

```bash
gotify-push -k <app_key_here> -t "Hello" -m "Hello World"
```

where `<app_key_here>` is the name of the application provided in the configuration file.
