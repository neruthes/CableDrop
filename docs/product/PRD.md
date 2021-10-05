# CableDrop Product Requirement Product





## Abstract

AirDrop has become a very successful feature on Mac OS X and iOS.

CableDrop is designed to serve similar features on GNU/Linux.




## Scenario

Alice would like to drop a file to Bob, who resides within the same LAN.

Both parties have `cabledropd` running as a service, listening on port `4592`.

The file being shared may be given from stdin or from filename.

Alice selects the file, then selects Bob.

The file is then securely transferred.




## Components

This software contains 2 programs:

- `cabledrop`
- `cabledropd`

### cabledrop

CLI frontend. Sender.

This program will spawn a HTTP server daemon.

### cabledropd

Desktop daemon (port 4592, HTTP). Receiver.

This program advertises this host to the local network.

This program makes prompts to the user when receiving incoming drop requests.

The lifecycle is equal to the desktop environment session.



## Basic Workflow

Alice executes `cabledrop img.png` or `cat img.png | cabledrop`. If no filename is given, the program shall assume that the string `stdin` will be used when a human-readable filename is needed.

The program shows a list of discovered recipients, where Bob is included.

Alice answers `3` (list index), or `Bob.lan` (local hostname), or `192.168.1.77` (local IP address), then types Return.

This operation is assigned a UUID ("Session ID"), e.g. `0626a519-c19f-45b9-afcc-47e97dac3359`.

The file is written (if stdin) or symlinked (if filename) into `/tmp/cabledrop-alice/0626a519-c19f-45b9-afcc-47e97dac3359/img.png`.

A pre-established HTTP server should be serving the file at `http://127.0.0.1:4579/0626a519-c19f-45b9-afcc-47e97dac3359/img.png`, where the port number may vary. If daemon is not running, the program shall immediately start a temporary HTTP server.

The program sends the URL to `192.168.1.77:4592`.

# Files

### `/etc/cabledrop.conf`

The program `cabledrop` will source this file when starting.

Variables include:

| Variable Name    | Example | Description                                      |
| ---------------- | ------- | ------------------------------------------------ |
| HTTP_SERVER_PORT | 4579    | The port which the HTTP server should listen     |
| HTTP_SERVER_TYPE | python3 | HTTP server type. Can be `python2` or `python3`. |

### `/tmp/.cabledrop-targets`

This file is maintained by `cabledropsd`.