# OpenRPZ Firewall — Installation Guide

## Supported beta build

- Ubuntu 26.04
- CPython 3.14
- BIND9

This is an experimental CLI release. Installation does not
automatically activate DNS filtering.

## 1. Install dependencies

sudo apt-get update
sudo apt-get install -y python3 bind9 bind9-utils dnsutils

## 2. Download the beta

Download the Linux archive and matching SHA256 file from:
https://github.com/emredes666/openrpz-firewall-releases/releases

Verify both files are in the same directory.

## 3. Verify integrity

sha256sum -c *.tar.gz.sha256

## 4. Extract and install

mkdir -p ~/openrpz-beta9
tar -xzf openrpz-firewall-1.0.0-beta.9.0-rc4-cpython314-linux.tar.gz \
  -C ~/openrpz-beta9

cd ~/openrpz-beta9

python3 tools/openrpz.pyc status
sudo bash install-compiled.sh

## 5. Verify installation

openrpz status

## Important

- Installation requires a fresh system without an existing OpenRPZ installation.
- The installer does not enable or restart BIND9.
- DNS setup and activation require additional configuration.
- Do not install directly over an existing production DNS service.
- Back up your system before performing DNS changes.
- The distributed Python bytecode is not protected against reverse engineering.
