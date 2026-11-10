# OpenRPZ Firewall — Beta Testing

## Environment information

Include the following in bug reports:

- Ubuntu version
- Python version
- OpenRPZ version
- BIND9 version
- Installation method

## CLI tests

python3 --version
named -v
openrpz status
openrpz dns --help
openrpz update --help

## DNS status

openrpz dns status \
  --server 192.168.1.100 \
  --zone test.rpz.local

Replace the address and zone with your lab configuration.

## Blocking test

After separately configuring and activating a test RPZ:

dig @192.168.1.100 example.com A

A blocked domain may return NXDOMAIN if configured with
the corresponding RPZ policy.

Compare against an authorized baseline resolver to ensure
the domain normally resolves.

Do not assume that NXDOMAIN alone proves RPZ blocking.

## Submit feedback

Report installation errors, unexpected DNS responses,
feed update issues and rollback problems via GitHub Issues.

Never submit passwords, tokens, private feeds or internal secrets.

## Current limitations

The public beta has not completed a clean-install end-to-end
test of automatic feed activation. DNS setup requires manual work.
