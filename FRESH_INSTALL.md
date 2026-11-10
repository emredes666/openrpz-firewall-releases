# OpenRPZ Firewall Beta 9 RC4 — Fresh Installation

## Important

This is an experimental beta release.

The installer installs the OpenRPZ CLI but does NOT
automatically activate DNS filtering, configure BIND9
or download and publish all blocklists.

Do not deploy directly to a production DNS server.

## Step 1 — Install Ubuntu Server

Recommended tested build target:
Ubuntu Server 26.04 with CPython 3.14.

Use a dedicated physical computer or virtual machine.

## Step 2 — Configure a static IP

Example network:

Router:         192.168.1.1
OpenRPZ Ubuntu: 192.168.1.10
Windows PC:     192.168.1.20

Assign Ubuntu a static address or configure
a DHCP reservation on your router.

Keep the default gateway pointed at the router.

## Step 3 — Install dependencies

    sudo apt update
    sudo apt install -y python3 bind9 bind9-utils dnsutils

Verify:

    python3 --version
    named -v

The cpython314 release requires CPython 3.14.

## Step 4 — Download OpenRPZ Firewall

Download the cpython314 archive and SHA256 file:

https://github.com/emredes666/openrpz-firewall-releases/releases

Keep both files in the same directory.

## Step 5 — Verify and install

Run from the download directory:

    sha256sum -c openrpz-firewall-1.0.0-beta.9.0-rc4-cpython314-linux.tar.gz.sha256

    mkdir -p ~/openrpz-beta9

    tar -xzf openrpz-firewall-1.0.0-beta.9.0-rc4-cpython314-linux.tar.gz -C ~/openrpz-beta9

    cd ~/openrpz-beta9

    python3 tools/openrpz.pyc status
    sudo bash install-compiled.sh
    openrpz status

The installer refuses to overwrite existing
OpenRPZ installations.

## Step 6 — Prepare DNS configuration

Review the installed example configuration:

    sudo ls -l /opt/openrpz-firewall/config/

Inspect available setup options:

    openrpz dns setup --help

Generate a standalone laboratory preview:

    openrpz dns setup --output "$HOME/openrpz-dns-preview" --zone test.rpz.local --listen-ip 127.0.0.1 --port 1053

This creates a preview only.
It does not enable DNS filtering.

## Step 7 — Activate BIND9 and RPZ

MANUAL CONFIGURATION REQUIRED.

An administrator must configure:

- BIND9 LAN listener and authorized clients
- DNS recursion and upstream resolution
- RPZ zone file and response-policy configuration
- File permissions and AppArmor compatibility
- DNS service activation and health checks

The current public beta does not automate these steps.

Do not run DNS activation or publishing commands
against a working DNS service without a backup
and verified rollback plan.

## Step 8 — Configure feeds and publish policy

MANUAL CONFIGURATION REQUIRED.

Supported feed families include OISD and HaGeZi.

Feed selection, exclusions, zone generation,
validation and publication must be configured
and tested before enabling blocking.

Check available commands:

    openrpz update --help
    openrpz dns publish --help

Automatic DNS publication is not enabled by default.

## Step 9 — Configure Windows DNS

After the DNS server is fully activated and verified,
set the Windows network adapter DNS to the Ubuntu IP.

Example:

Preferred DNS: 192.168.1.10

The default gateway remains the router: 192.168.1.1.

Check DNS:

    ipconfig /flushdns
    nslookup openai.com 192.168.1.10

Check the default resolver:

    nslookup openai.com

Verify IPv6 DNS settings, browser Secure DNS / DoH
and VPN settings are not bypassing OpenRPZ.

## Step 10 — Test DNS filtering

Check a known domain covered by your active RPZ:

    nslookup BLOCKED_DOMAIN 192.168.1.10

Expected for NXDOMAIN-style policies:

    Non-existent domain

Compare with a resolver without the RPZ policy.

Then open:

https://blockmeter.io/

A successful Blockmeter result depends on
the enabled lists, client DNS configuration
and browser behavior.

A score of 100% is not guaranteed.

## Troubleshooting

Check:

    openrpz status
    openrpz dns --help
    openrpz update --help
    sudo named-checkconf

For further guidance, see:

- INSTALL.md
- COMMANDS.md
- NETWORK_SETUP.md
- TROUBLESHOOTING.md

## Known Beta 9 RC4 limitations

- No automatic DNS activation by the installer
- No automatic first-run feed publication
- Clean Ubuntu installation testing is incomplete
- Python bytecode can be reverse-engineered
- Not recommended for production environments

Report bugs using GitHub Issues.
Never publish passwords, tokens or private config files.

---

## Beta 9 RC4 — End-to-End Installation Test Plan

This section describes the required validation process
for a fresh OpenRPZ Firewall Beta 9 RC4 installation.

### Phase 1 — Fresh Ubuntu Server 26.04 Installation

- Install Ubuntu Server 26.04 on a dedicated VM or physical server.
- Assign a static LAN IP address or DHCP reservation.
- Configure the router as the default gateway.
- Verify Internet connectivity and DNS resolution.
- Confirm Python 3.14 and BIND9 availability.

### Phase 2 — OpenRPZ Firewall Beta 9 RC4 Installation

- Download the CPython 3.14 release package from GitHub Releases.
- Download and verify the matching SHA256 checksum.
- Extract the release archive.
- Run the installation script.
- Verify the CLI using `openrpz status`.
- Confirm existing DNS installations are not overwritten.

### Phase 3 — BIND9 and Six Feed Sources

- Configure BIND9 to listen on the intended LAN IP.
- Restrict recursive DNS access to trusted clients.
- Configure and activate the RPZ policy zone.
- Configure the six feed sources used by the lab:
  OISD Big, HaGeZi PRO, HaGeZi TIF Full 1,
  HaGeZi TIF Full 2, HaGeZi SafeSearch,
  and HaGeZi DoH/VPN/Tor/Proxy.
- Download and validate the selected feeds.
- Apply exclusions and deduplicate domains.
- Generate and validate the RPZ zone.
- Publish only after backups and health checks.
- Verify blocked and allowed domains.
- Test recovery using a controlled failure.

IMPORTANT: Beta 9 RC4 does not automatically perform
all of these steps. Manual DNS activation and feed
configuration are required.

### Phase 4 — Windows DNS and Blockmeter Testing

- Configure the Windows PC to use the Ubuntu OpenRPZ IP
  as its preferred DNS resolver.
- Keep the router as the default gateway.
- Flush the Windows DNS cache.
- Verify that the default resolver is OpenRPZ.
- Check blocked domains return NXDOMAIN.
- Check allowed domains resolve normally.
- Verify browser Secure DNS / DoH does not bypass OpenRPZ.
- Run https://blockmeter.io/ and record the results.

### Phase 5 — Documentation and Bug Reporting

- Record installation errors and unexpected DNS responses.
- Document any manual steps missing from this guide.
- Verify changes against a fresh installation.
- Update INSTALL.md, COMMANDS.md and NETWORK_SETUP.md
  where necessary.
- Submit reproducible issues through GitHub Issues.
- Never disclose credentials, private feeds or secrets.

### Validation Status

The CPython 3.14 CLI, package checksum, RPZ configuration
generation, and isolated DNS blocking have been tested.

A complete fresh Ubuntu installation with automatic
six-feed activation is not yet verified.

Do not treat this checklist as proof that all phases passed.
