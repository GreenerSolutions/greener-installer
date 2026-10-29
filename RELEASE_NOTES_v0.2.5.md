# Greener Installer v0.2.5

Use this release instead of v0.2.4 for fresh Ubuntu 26.04 amd64 installations.

This release fixes the fresh-install dependency between ChirpStack Backend
startup and API-token publication. The token is published before the service
starts, and both gateway validation and the subsequent secret handoff enforce
exact file contents, ownership, permissions, integrity and resumable state.
No manual secret copying or journal edits are required.

New guided installations explicitly publish frontend HTTPS on all IPv4 host
interfaces, including the building VPN, using `network.frontend_bind: 0.0.0.0`.
Existing configurations that omit this setting retain their previous frontend
bind address. The BACnet protocol bind and TLS certificate inputs are unchanged.
Keep building application ports behind the site firewall; do not forward them
from the public Internet.

The bundle contains Greener Building Server 0.2.5-1 and the exact tested
Tailscale 1.104.2 amd64 package. Production defaults remain
`nexus.smarterbuildings.ca`, `connect.smarterbuildings.ca`, and
`registry.smarterbuildings.ca`. Download all seven assets and verify SHA256SUMS.

Validation includes 1,038 installer tests (10 skipped), a separately executed
root-only volume ownership regression, independent review, clean Ubuntu 26.04
package build, extracted-package audit, installed-package smoke testing, and
real clean-host acceptance against the production control plane. Acceptance
covers first-login password replacement, eight-container telemetry, actual
operator VPN traffic, and automatic backend/frontend recovery.

Package installation, activation and applying a service release remain separate
operations. After completing activation, `sudo greener update --check-only`
reports the installed application release to Nexus without applying an update.
This bundle contains no application images, enrollment keys or site credentials.
