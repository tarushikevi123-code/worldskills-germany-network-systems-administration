# Device configurations

Place sanitized configuration exports and command notes here, one file per device (for example, `Weil_A1.txt`). Include a short header describing the device role and the date / scenario to which the export applies.

Before committing, remove or redact:

- `username ... secret` and `enable secret` lines;
- clear-text credentials and password hashes;
- RSA/SSH private keys and certificate private-key material;
- SNMP communities, API keys, VPN pre-shared keys, and tokens;
- personal, institutional, or non-public addressing and identifying information.

Prefer a focused configuration extract when a full `show running-config` is not needed. Keep original unredacted backups outside the public repository.
