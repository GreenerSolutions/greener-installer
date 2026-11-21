# Greener installer v0.3.6

Package: `greener-building-server_0.3.6-1_amd64.deb` for fresh Ubuntu 26.04 amd64 hosts.

## Per-interface Ethernet setup

Guided setup detects physical Ethernet ports, including USB adapters and unplugged
ports. Each port can be enabled or disabled. Enabled ports offer Keep, DHCP, or
Static addressing; static addresses and prefixes must be entered explicitly.
Choose a standard purpose or type a custom purpose label. Labels are descriptive;
Internet/default routing and the BACnet/building interface are selected separately.

The installer reviews the complete selection before changing networking. Changes
use a 120-second trial with an independent rollback timer. Confirm after applying,
including after reconnecting if SSH disconnects. Unconfirmed changes restore the
saved network configuration. Root-owned recovery state supports interrupted setup.
Nexus enrollment and application setup wait for network confirmation.

To inspect or confirm a pending trial after reconnecting:

```sh
sudo greener network-trial status
sudo greener network-trial confirm --id TRIAL_ID
# To restore the previous network instead:
sudo greener network-trial rollback --id TRIAL_ID
```

After confirmation, resume with `sudo greener setup --guided`.

## Compatibility and scope

This release is for new installations. It does not automatically update existing
buildings, change application image releases, or migrate existing installations.
Production Nexus, registry and VPN endpoints remain unchanged. Tailscale 1.104.2
is bundled with its pinned checksum. No credentials or production configuration
are included in the public release.

## Verification limits

Automated network/setup checks, isolated Netplan generation and static systemd
verification passed. Live multi-interface SSH disconnect, timeout recovery and
reboot acceptance for this new network flow are explicitly deferred by the
operator; this release does not claim those checks have passed. Existing v0.3.4
host acceptance does not establish acceptance of the new trial implementation.

## Release verification

- GitHub Ubuntu 26.04 validation: 1,310 source tests passed (three conditional
  checks skipped), 61 installed-package tests and four updater tests passed.
- Clean Ubuntu 26.04 package build and extracted-package audit passed. The
  published package also passed an isolated install, CLI/status smoke and static
  verification of all four installed network-trial units.
- 236 relevant milestone tests passed; the corrected local release harness
  passed 186 affected tests. An initial local full-suite run used unsafe temporary
  mount permissions and is invalid evidence; authoritative full-suite results
  are the successful GitHub run.
- Four real Netplan generation cases and independent source review passed.
  Review's rollback journal ordering issue was fixed and rechecked.
- All seven bundle assets were checked against their checksums before publication.

Debian package SHA-256:
`fc90506f25abbe983a8a821351d880a3cb3dd83b93eb64818788f1029baf545a`.

Source: [GreenerSolutions/install PR #23](https://github.com/GreenerSolutions/install/pull/23).
Build source commit: `13f7f442fb22a90a93a9a5b856d5897938741b1c`.
