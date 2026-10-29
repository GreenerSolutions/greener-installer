# Greener Installer v0.2.4

Superseded by v0.2.5 for fresh installations. A subsequent empty-state live test
exposed a ChirpStack Backend startup/token ordering failure in v0.2.4; use the
corrected release. Existing immutable v0.2.4 assets have not been replaced.

New installations default to the AWS control plane at
`nexus.smarterbuildings.ca`, `connect.smarterbuildings.ca`, and
`registry.smarterbuildings.ca`.

Existing local installations retain their VPN endpoint when upgrading, even
when an older configuration omits `nexus.vpn_url`. Guided setup now records
the VPN URL explicitly. Operator-specified URLs remain supported.

The supported building host is Ubuntu 26.04 amd64. The release includes
Greener Building Server 0.2.4-1 and the exact tested Tailscale 1.104.2 amd64
package. Download all seven assets and verify `SHA256SUMS` before installing.

Validation includes the full installer suite (1,022 tests, 9 skipped), a clean
package build and installed-package smoke test in GitHub Actions, the package
audit, and final bundle checksum verification. This release contains no
application images, reusable enrollment keys, or deployment credentials.
