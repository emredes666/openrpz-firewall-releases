# OpenRPZ Firewall — Home Network Setup

## 1. Network topology

OpenRPZ Firewall operates as a DNS filtering server.
It does not need to be physically installed between
your router and computer.

Example configuration:

| Device | IP address | Gateway | DNS |
|---|---|---|---|
| Router | 192.168.1.1 | - | - |
| Ubuntu OpenRPZ | 192.168.1.10 | 192.168.1.1 | Upstream resolver |
| Windows PC | 192.168.1.20 | 192.168.1.1 | 192.168.1.10 |

All addresses above are examples. Adapt them to your network.

## 2. Ubuntu server

Install Ubuntu Server on a dedicated VM or physical machine.

Assign a static IP address or use a DHCP reservation.

Ensure:
- Ubuntu can reach its upstream DNS resolvers.
- BIND9 is configured to listen on the Ubuntu LAN IP.
- The RPZ zone is loaded and activated.
- UDP and TCP port 53 are reachable from authorized clients.
- Recursion is restricted to trusted local networks.

IMPORTANT: Beta 9 RC4 installs the OpenRPZ CLI only.
The public installer does not automatically activate DNS,
download all feeds, or configure a working LAN resolver.

DNS activation must be completed separately and verified
in an isolated environment before real-world use.

## 3. Windows DNS configuration

Configure the Windows network adapter to use
the OpenRPZ Ubuntu IP as its DNS server.

Example:

Preferred DNS: 192.168.1.10

Keep the normal router IP as the default gateway.

Check the active DNS settings:

    Get-DnsClientServerAddress

Flush the DNS cache:

    ipconfig /flushdns

Test the OpenRPZ server directly:

    nslookup openai.com 192.168.1.10

For blocked domains, NXDOMAIN is expected when the
domain is included in an active RPZ blocking policy.

Check the default resolver:

    nslookup openai.com

If Windows still uses another DNS server, such as
an IPv6 router address, OpenRPZ may be bypassed.

Browser Secure DNS / DNS-over-HTTPS and VPN clients
may also bypass the system DNS resolver.

## 4. Blockmeter test

Open:

https://blockmeter.io/

Run the test from a computer configured to use OpenRPZ.

Blockmeter checks requests to multiple advertising,
tracking and other blocklist domains.

For a DNS-focused comparison:
- Confirm Windows is actually using OpenRPZ.
- Temporarily disable browser ad-blocking extensions.
- Configure browser Secure DNS to use the system resolver.
- Confirm the site's control domains resolve normally.
- Compare the result before and after enabling OpenRPZ.

Do not interpret the score as a security certification.
A score of 100% is not required or necessarily desirable.
Results depend on the enabled feed categories.

DNS filtering cannot block every advertisement,
especially ads delivered from the same domain as content.

## 5. Troubleshooting

If blocking does not work:
1. Confirm BIND9 is running.
2. Verify the correct LAN IP and port 53.
3. Check that the RPZ policy zone is active.
4. Inspect the blocked-domain records.
5. Check Windows DNS and IPv6 DNS settings.
6. Check browser DoH and VPN settings.
7. Verify DNS recursion and upstream connectivity.
8. Inspect BIND9 logs for errors.

Do not disable AppArmor or DNSSEC as a general
troubleshooting workaround.

## 6. Safety

Use an isolated lab for initial beta testing.

Do not expose an unrestricted recursive resolver
to the public Internet.

Do not replace production DNS services without
backups, testing and a recovery plan.

For bug reports, share sanitized CLI outputs,
DNS lookup results, Ubuntu version and BIND9 version.

Never share passwords, tokens or private configurations.
