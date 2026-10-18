# Security policy

Report vulnerabilities privately through GitHub's "Report a vulnerability" button on the Security tab. Do not open a public issue. We aim to acknowledge reports within 3 working days.

Supported versions: the latest major tag (`v1`).

The action downloads the `rdesk` binary from the `rdesk-cli` update feed on releasedesk.dev (override with `rdesk-feed`) and verifies it against the SHA-256 in that feed. The action always uses the latest stable build, and the checksum comes from the same origin as the file, so it detects corruption but not a compromised feed. Never print your API token in workflow logs.
