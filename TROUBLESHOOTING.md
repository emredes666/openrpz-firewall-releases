# OpenRPZ Firewall — Troubleshooting

## Command not found

Run:

openrpz status

Check whether installation completed successfully and
/usr/local/bin/openrpz exists.

## Python version mismatch

Check:

python3 --version

The cpython314 build requires CPython 3.14.
Use the build matching your supported environment.

## DNS status requires arguments

Provide the DNS server and zone explicitly:

openrpz dns status \
  --server 192.168.1.100 \
  --zone test.rpz.local

## DNS query returns SERVFAIL

Check:

- Upstream resolver connectivity
- BIND9 logs
- RPZ zone validity
- DNS forwarding and DNSSEC configuration

Do not disable DNSSEC in production merely to bypass errors.

## DNS configuration permission denied

Check file ownership, parent directory permissions
and AppArmor denials.

Do not disable AppArmor or broadly open file permissions.

## Blocking does not work

Check:

- Client DNS server address
- RPZ zone activation
- Actual blocked-domain records
- Browser Secure DNS / DNS-over-HTTPS settings
- IPv6 DNS resolver selection

## Existing installation detected

The beta installer deliberately refuses to overwrite an
existing OpenRPZ installation.

Do not delete an existing installation to force an upgrade.
Use a separate lab system instead.
