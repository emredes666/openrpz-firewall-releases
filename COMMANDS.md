# OpenRPZ Firewall — CLI Command Reference

## Basic commands

| Command | Description |
|---|---|
| `openrpz --help` | List main commands |
| `openrpz status` | Display application version and mode |
| `openrpz config --help` | Show configuration commands |
| `openrpz dns --help` | Show DNS management commands |
| `openrpz update --help` | Show feed update options |

## DNS commands

### Check DNS status

openrpz dns status --server 192.168.1.100 --zone test.rpz.local

Replace the example server address and zone with your own values.

### Generate a standalone DNS configuration preview

openrpz dns setup \
  --output "$HOME/openrpz-lab-dns" \
  --zone test.rpz.local \
  --listen-ip 127.0.0.1 \
  --port 1053

This generates a preview configuration. It does not activate BIND9.

### Inspect publishing options

openrpz dns publish --help

### Inspect DNS activation options

openrpz dns activate --help

## Feed updates

openrpz update --help

## Safety precautions

Commands involving `--apply`, DNS activation, or publication
may change the live DNS environment.

Use them only in an isolated lab after reviewing the options,
configuration, health checks and recovery procedure.

Automatic publication is not enabled by the beta installer.
