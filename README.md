# Community Hass.io Add-ons: Node-RED

[![GitHub Release][releases-shield]][releases]
![Project Stage][project-stage-shield]
[![License][license-shield]](LICENSE.md)

![Supports aarch64 Architecture][aarch64-shield]
![Supports amd64 Architecture][amd64-shield]
![Supports armhf Architecture][armhf-shield]
![Supports armv7 Architecture][armv7-shield]
![Supports i386 Architecture][i386-shield]

[![GitLab CI][gitlabci-shield]][gitlabci]
![Project Maintenance][maintenance-shield]
[![GitHub Activity][commits-shield]][commits]

[![Discord][discord-shield]][discord]
[![Community Forum][forum-shield]][forum]

[![Buy me a coffee][buymeacoffee-shield]][buymeacoffee]

[![Support my work on Patreon][patreon-shield]][patreon]

Flow-based programming for the Internet of Things.

![Node-RED in the Home Assistant Frontend](images/screenshot.png)

## About

Node-RED is a programming tool for wiring together hardware devices,
APIs and online services in new and interesting ways.

It provides a browser-based editor that makes it easy to wire together flows
using the wide range of nodes in the palette that can be deployed to its
runtime in a single click.

## Installation

The installation of this add-on is pretty straightforward and not different in
comparison to installing any other Hass.io add-on.

1. Search for the "Node-RED" add-on in the Hass.io add-on store and install it.
1. Set a `credential_secret`, which is used to encrypt sensitive data.
   This is just a "password", which you should save in a secondary location.
1. Start the "Node-RED" add-on.
1. Check the logs of "Node-RED" to see if everything went well.
1. Click on the "OPEN WEB UI" button to jump into Node-RED.
1. The add-on works straight out the box! No need to configure a server!

**Note**: The add-on is **pre-configured** out of the box! There is no need
to add/change/update the server connection settings!

Please read the rest of this document further instructions.

## Configuration

**Note**: _Remember to restart the add-on when the configuration is changed._

Example add-on configuration:

```json
{
  "log_level": "info",
  "credential_secret": "KJHhfdhiFRENCKfsdfdsDHFHDJS",
  "http_node": {
    "username": "MarryPoppins",
    "password": "Supercalifragilisticexpialidocious"
  },
  "http_static": {
    "username": "MarryPoppins",
    "password": "Supercalifragilisticexpialidocious"
  },
  "ssl": true,
  "certfile": "fullchain.pem",
  "keyfile": "privkey.pem",
  "require_ssl": true,
  "system_packages": [
    "ffmpeg"
  ],
  "npm_packages": [
    "node-red-admin"
  ],
  "init_commands": [
    "echo 'This is a test'",
    "echo 'So is this...'"
  ]
}
```

**Note**: _This is just an example, don't copy and paste it! Create your own!_

### Option: `log_level`

The `log_level` option controls the level of log output by the addon and can
be changed to be more or less verbose, which might be useful when you are
dealing with an unknown issue. Possible values are:

- `trace`: Show every detail, like all called internal functions.
- `debug`: Shows detailed debug information.
- `info`: Normal (usually) interesting events.
- `warning`: Exceptional occurrences that are not errors.
- `error`:  Runtime errors that do not require immediate action.
- `fatal`: Something went terribly wrong. Add-on becomes unusable.

Please note that each level automatically includes log messages from a
more severe level, e.g., `debug` also shows `info` messages. By default,
the `log_level` is set to `info`, which is the recommended setting unless
you are troubleshooting.

### Option: `ssl`

Enables/Disables SSL (HTTPS) on the web interface.
Set it `true` to enable it, `false` otherwise.

**Note**: _The SSL settings only apply to direct access and has no effect
on the Hass.io Ingress service._

### Option: `certfile`

The certificate file to use for SSL.

**Note**: _The file MUST be stored in `/ssl/`, which is the default for Hass.io_

### Option: `keyfile`

The private key file to use for SSL.

**Note**: _The file MUST be stored in `/ssl/`, which is the default for Hass.io_

### Option: `require_ssl`

This option can be used to cause insecure HTTP connections to be redirected
to HTTPS. This is recommended when you have SSL enabled.

### Option: `credential_secret`

Credentials are encrypted by Node-RED in storage, using a secret key.
This option allows you to specify your secret key. This can be anything
you like, it is just like a password. Be sure to store it somewhere safe.
You might need it in the future! (e.g., When restoring a backup).

**Note**: _Once you set this property, do not change it - doing so will prevent
Node-RED from being able to decrypt your existing credentials and they will be
lost._

**Note**: _This option support secrets, e.g., `!secret red_secret`._

### Option: `dark_mode`

When set to `true`, the Midnight Node-RED theme by [Mauricio Bonani][bonanitech]
will be enabled. For more information and a glance at how it looks,
see the GitHub repository of this theme:

<https://github.com/bonanitech/node-red-contrib-theme-midnight-red>

### Option: `http_node`

To password protect the node-defined HTTP endpoints (`httpNodeRoot`),
the following properties can be used:

- `username`
- `password`

**Note**: _These options support secrets, e.g., `!secret red_password`._

**Note**: _In order to use the `http_node` you will need to expose Node-RED using
a network port in addition to ingress. The HTTP nodes will also be presented
under `/endpoint/` as shown in the UI. If using the `node-red-dashboard` module
this will also be hosted under this path and will use any credentials set here._

### Option: `http_static`

To password protect the static content (httpStatic), the following
properties can be used:

- `username`
- `password`

**Note**: _These options support secrets, e.g., `!secret red_password`._

### Option: `system_packages`

Allows you to specify additional [Alpine packages][alpine-packages] to be
installed to your Node-RED setup (e.g., `g++`. `make`, `ffmpeg`).

**Note**: _Adding many packages will result in a longer start-up time
for the add-on._

### Option: `npm_packages`

Allows you to specify additional [NPM packages][npm-packages] or
[Node-RED nodes][node-red-nodes] to be installed to your Node-RED setup
(e.g., `node-red-dashboard`, `node-red-contrib-ccu`).

**Note**: _Adding many packages will result in a longer start-up time
for the add-on._

### Option: `init_commands`

Customize your Node-RED environment even more with the `init_commands` option.
Add one or more shell commands to the list, and they will be executed every
single time this add-on starts.

### Option: `i_like_to_be_pwned`

Adding this option to the add-on configuration allows to you bypass the
HaveIBeenPwned password requirement by setting it to `true`.

**Note**: _We STRONGLY suggest picking a stronger/safer password instead of
using this option! USE AT YOUR OWN RISK!_

### Option: `leave_front_door_open`

Adding this option to the add-on configuration allows you to disable
authentication on the add-on by setting it to `true` and leaving the
username and password empty.

**Note**: _We STRONGLY suggest, not to use this, even if this add-on is
only exposed to your internal network. USE AT YOUR OWN RISK!_

## Time zone configuration

The addon will use the configured time zone of the underlying operating system.
If this is incorrect (for example with HassOS this will be UTC), this can be
configured in the `/config/node-red/settings.js` file.

To do so, open the file with a text editor and add the following above the
`module.exports = {` line.

`process.env.TZ = "America/Toronto";`

The time zone will need to reflect your environment.

Save the file and restart the Node-RED add-on.

## Known issues and limitations

- While this add-on ships with Node-RED Dashboard, it currently does not
  support accessing the dashboard via Hass.io Ingress. This is a technical
  limitation on the Node-RED Dashboard end.

- If you cannot access HTTP nodes or Node-RED Dashboard, please check
  if you have enabled direct access mode by setting a port number in
  "Network" configuration section of the add-on.

- If you cannot access HTTP nodes or Node-RED Dashboard, please check
  if you URL starts with `/endpoint/`, or else Home Assistant authentication
  will kick in.

## Changelog & Releases

This repository keeps a change log using [GitHub's releases][releases]
functionality. The format of the log is based on
[Keep a Changelog][keepchangelog].

Releases are based on [Semantic Versioning][semver], and use the format
of ``MAJOR.MINOR.PATCH``. In a nutshell, the version will be incremented
based on the following:

- ``MAJOR``: Incompatible or major changes.
- ``MINOR``: Backwards-compatible new features and enhancements.
- ``PATCH``: Backwards-compatible bugfixes and package updates.

## Support

Got questions?

You have several options to get them answered:

- The [Community Hass.io Add-ons Discord chat server][discord] for add-on
  support and feature requests.
- The [Home Assistant Discord chat server][discord-ha] for general Home
  Assistant discussions and questions.
- The Home Assistant [Community Forum][forum].
- Join the [Reddit subreddit][reddit] in [/r/homeassistant][reddit]

You could also [open an issue here][issue] GitHub.

## Contributing

This is an active open-source project. We are always open to people who want to
use the code or contribute to it.

We have set up a separate document containing our
[contribution guidelines](CONTRIBUTING.md).

Thank you for being involved! :heart_eyes:

## Authors & contributors

The original setup of this repository is by [Franck Nijhof][frenck].

For a full list of all authors and contributors,
check [the contributor's page][contributors].

## We have got some Hass.io add-ons for you

Want some more functionality to your Hass.io Home Assistant instance?

We have created multiple add-ons for Hass.io. For a full list, check out
our [GitHub Repository][repository].

## License

MIT License

Copyright (c) 2018-2019 Franck Nijhof

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

[aarch64-shield]: https://img.shields.io/badge/aarch64-yes-green.svg
[alpine-packages]: https://pkgs.alpinelinux.org/packages
[amd64-shield]: https://img.shields.io/badge/amd64-yes-green.svg
[armhf-shield]: https://img.shields.io/badge/armhf-yes-green.svg
[armv7-shield]: https://img.shields.io/badge/armv7-yes-green.svg
[bonanitech]: https://github.com/bonanitech
[buymeacoffee-shield]: https://www.buymeacoffee.com/assets/img/guidelines/download-assets-sm-2.svg
[buymeacoffee]: https://www.buymeacoffee.com/frenck
[commits-shield]: https://img.shields.io/github/commit-activity/y/hassio-addons/addon-node-red.svg
[commits]: https://github.com/hassio-addons/addon-node-red/commits/master
[contributors]: https://github.com/hassio-addons/addon-node-red/graphs/contributors
[discord-ha]: https://discord.gg/c5DvZ4e
[discord-shield]: https://img.shields.io/discord/478094546522079232.svg
[discord]: https://discord.me/hassioaddons
[forum-shield]: https://img.shields.io/badge/community-forum-brightgreen.svg
[forum]: https://community.home-assistant.io/t/community-hass-io-add-on-node-red/55023?u=frenck
[frenck]: https://github.com/frenck
[gitlabci-shield]: https://gitlab.com/hassio-addons/addon-node-red/badges/master/pipeline.svg
[gitlabci]: https://gitlab.com/hassio-addons/addon-node-red/pipelines
[home-assistant]: https://home-assistant.io
[i386-shield]: https://img.shields.io/badge/i386-yes-green.svg
[issue]: https://github.com/hassio-addons/addon-node-red/issues
[keepchangelog]: http://keepachangelog.com/en/1.0.0/
[license-shield]: https://img.shields.io/github/license/hassio-addons/addon-node-red.svg
[maintenance-shield]: https://img.shields.io/maintenance/yes/2019.svg
[node-red-nodes]: https://flows.nodered.org/?type=node&num_pages=1
[npm-packages]: https://www.npmjs.com
[patreon-shield]: https://www.frenck.nl/images/patreon.png
[patreon]: https://www.patreon.com/frenck
[project-stage-shield]: https://img.shields.io/badge/project%20stage-production%20ready-brightgreen.svg
[reddit]: https://reddit.com/r/homeassistant
[releases-shield]: https://img.shields.io/github/release/hassio-addons/addon-node-red.svg
[releases]: https://github.com/hassio-addons/addon-node-red/releases
[repository]: https://github.com/hassio-addons/repository
[semver]: http://semver.org/spec/v2.0.0.htm
