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

Existing hosts need a one-time installation of the `0.3.7-1` package and the
trusted installer-package public policy before they can poll Nexus for upgrades.
Schedule subsequent upgrades from **Installations → Installer upgrades**.

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

## Release verification

- GitHub Ubuntu 26.04 validation completed with 1,323 passing source tests,
  three conditional skips, 61 installed-package tests, and four updater tests.
- A disposable Ubuntu host verified and installed the signed package from
  `0.3.6-1` to `0.3.7-1` and reported success. Bundle checksums, package audit,
  static systemd checks, and independent source review passed.
- Live building upgrades and Ethernet disconnect and reboot acceptance are
  deferred.

Debian package SHA-256:
`13599675aa1b44374a7e18fd682ec9ea11d3a85858eda8026ab2d5e965c63a8f`.

Source: [GreenerSolutions/install PR #25](https://github.com/GreenerSolutions/install/pull/25).
Build source commit: `0f2e153fc695f5ab57ca213e672d6e022a0436e7`.
