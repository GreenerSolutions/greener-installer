# Greener installer v0.3.7

Package: `greener-building-server_0.3.7-1_amd64.deb` for fresh Ubuntu 26.04
amd64 hosts.

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

## Managed runtime settings

The transactional service-profile Apply flow can manage an allowlisted set of
nonsecret runtime settings for the OCPP Proxy and ChirpStack backend. Existing
configuration is preserved when upgrading older files, and validation uses the
effective settings before a profile is applied. Passwords, tokens, credentials,
and host wiring are outside this feature and are not included in the release.

## Signed installer package upgrades

Nexus can approve exact-version installer package upgrades using a signed
`installer-release.json` approval input. This input is separate from the seven
bootstrap bundle assets and contains no credentials. The upgrade path accepts
only the verified Greener package identified by its version, architecture, size,
and SHA-256 digest.

This is a centrally coordinated rollout. Pre-existing hosts need a one-time
coordinated installation of the `0.3.7-1` package and provisioned release trust
before upgrade polling can be enabled. These notes make no claim that any live
building host has already been upgraded.

## Compatibility and scope

This release supports fresh Ubuntu 26.04 amd64 installations. It does not
automatically change application image releases or migrate existing
installations. Production Nexus, registry, and VPN endpoints remain unchanged.
Tailscale 1.104.2 is bundled with its pinned checksum. No credentials or
production configuration are included in the public release.

Each immutable bootstrap bundle contains exactly these seven assets:

- `greener-building-server_0.3.7-1_amd64.deb`
- `tailscale_1.104.2_amd64.deb`
- `tailscale-package.json`
- `install-release-bundle.sh`
- `installer-entrypoint.json`
- `release-manifest.json`
- `SHA256SUMS`

## Verification limits

Clean bundle and checksum verification passed. An isolated Ubuntu worker upgrade
from `0.3.6-1` to `0.3.7-1` and static systemd verification passed, followed by
independent review. The broad GitHub CI run was still in progress when these
notes were prepared; no GitHub CI pass is claimed here. Live building-host
upgrade, disconnect, and reboot acceptance are not claimed by this release note.

Debian package SHA-256:
`13599675aa1b44374a7e18fd682ec9ea11d3a85858eda8026ab2d5e965c63a8f`.

Source: [GreenerSolutions/install PR #25](https://github.com/GreenerSolutions/install/pull/25).
Candidate build source commit: `0f2e153fc695f5ab57ca213e672d6e022a0436e7`.
