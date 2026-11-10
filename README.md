# OpenRPZ Firewall

DNS-based ad, tracker and threat-domain blocking using BIND9 RPZ.

## Beta 9 RC4 — Experimental Preview

- Ubuntu 26.04 / Python 3.14
- OISD and HaGeZi feed support
- RPZ configuration generation and validation
- Feed update preview, publication safeguards and rollback
- Command-line interface

## Download

Get the experimental build from GitHub Releases.

## Important limitations

This is not a production-ready firewall.
DNS activation and feed configuration require manual setup.
Automatic publishing is not enabled by default.
The Python bytecode can be reverse-engineered.

Report problems through GitHub Issues.
Never share credentials or private configuration files.

## Home network setup

See [NETWORK_SETUP.md](NETWORK_SETUP.md) for router, Ubuntu, Windows DNS and Blockmeter testing instructions.

## Fresh installation guide

See [FRESH_INSTALL.md](FRESH_INSTALL.md) for the
10-step Ubuntu installation, DNS setup and Windows testing guide.
