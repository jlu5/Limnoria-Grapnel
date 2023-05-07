# Grapnel hooks for Limnoria

<a href="https://drone.overdrivenetworks.com/jlu5/Limnoria-Grapnel" style="float: right">
  <img src="https://drone.overdrivenetworks.com/api/badges/jlu5/Limnoria-Grapnel/status.svg" />
</a>

**Grapnel** allows announcing Slack-style webhooks to IRC channels, via Limnoria's HTTP server. Currently, only plain text payloads are supported.

## Install

### Via pip

You can install Limnoria plugins using `pip`:

```
$ pip3 install "git+https://github.com/jlu5/Limnoria-Grapnel.git#egg=Grapnel"
```

### Via Git checkout

Or, clone the Git repository somewhere and add the *parent* directory to your bot's `config directories.plugins` setting.
Make sure the directory that you save the plugin to matches the plugin name:

```
$ git clone https://github.com/jlu5/Limnoria-Grapnel Grapnel
```

After cloning, you can update your copy of the plugin in place by entering the Grapnel directory and running:

```
$ git pull
```

## Usage

1) Configure the public base URL of your web server: `config plugins.grapnel.baseurl https://address.of.your.bot`

2) Create a webhook: `grapnel add [network] [channel]`
    - `network` and `channel` default to the current network and channel, if you run this command in a channel.

3) Save the URL generated and plug it into whatever app you want to announce events from.
    - Replace `your-cool-app-name` with the name of the sending app; it will be reflected in the output by default
    - **Make sure to keep this link private, as it includes the token!**

4) Test the webhook: e.g.

```bash
$ curl -X POST -d '{"text": "hello world"}' -H 'Content-type: application/json' 'https://address.of.your.bot/grapnel/1?token=xxxxxx&sender=myapp'
```

### Command index

- **`grapnel add [<network>] [<channel>]`** -- Creates a new Slack-compatible webhook endpoint for a given network + channel. `network` and `channel` default to the current network and channel if not specified.
- **`grapnel get <webhook ID>`** -- Returns metadata and the URL for the webhook ID.
- **`grapnel listhooks [<network>] [<channel>]`** -- Lists webhooks set on the network + channel pair. `network` and `channel` default to the current network and channel if not specified.
- **`grapnel remove <webhook ID>`** -- Removes the given webhook ID.
- **`grapnel resettoken <webhook ID>`** -- Regenerates the token for the given webhook ID.

### Output template

The `plugins.grapnel.format` option can be adjusted to change the output format. It supports these fields:

- `$name` (the sender name from the `sender=` part of the URL)
- `$text`
- Limnoria's standard substitutions:
    - `$randomInt`
    - `$botnick`
    - `$today`
    - `$now`
    - `$randomDate`
