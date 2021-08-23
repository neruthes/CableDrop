# CableDrop Product Requirement Product

## Abstract

AirDrop has become a very successful feature on Mac OS X and iOS.

CableDrop is designed to serve similar features on GNU/Linux.

## Scenario

Alice would like to drop a file to Bob, who resides within the same LAN.

Both parties have cabledropd running as a service, listening on any port from 4570 to 4580.

The file being shared may be given from stdin or from filename.

Alice selects the file, then selects Bob.

The file is then securely transferred.

## Workflow

Alice executes `cabledrop img.png` or `cat img.png | cabledrop`. If no filename is given, the program shall assume that the string `stdin` will be used when a human-readable filename is needed.

The program shows a list of discovered recipients, where Bob is included.

Alice answers `3` (list index) or `Bob.lan` (local hostname), then types Return.

This operation is assigned a UUID, e.g. `0626a519-c19f-45b9-afcc-47e97dac3359`.

The file is written (if stdin) or symlinked (if filename) into `/tmp/cabledrop-alice/0626a519-c19f-45b9-afcc-47e97dac3359/img.png`.

A pre-established HTTP server should be serving the file at `http://127.0.0.1:14570/0626a519-c19f-45b9-afcc-47e97dac3359/img.png`, where the port number may vary. If daemon is not running, the program shall immediately start a temporary HTTP server.

// TODO

## Details

### Beacon Lifecycle

// TODO