# Greener installer v0.3.4

Debian package: `greener-building-server_0.3.4-1_amd64.deb`.
Target: fresh Ubuntu 26.04 amd64 machines. Application release `0.2.11` and its
nine immutable image references remain unchanged.

## New-install release reporting

The installer reports its verified local application release during ordinary
Nexus heartbeats. Fresh installations no longer need a manual update check to
show the current release. The root reconciler supplies the bounded observation;
the unprivileged heartbeat agent does not read private manifests or Docker.

## Manifest authenticity

Initial enrollment previews, explicit setup manifests and new update targets
require Ed25519 signatures. The package independently provisions the production
PUBLIC verification key at `/etc/greener/trusted-manifest-keys.json` as a
root-owned Debian configuration file. It contains no signing private key or
credential, and operator modifications are preserved by package management.

The current production public-key fingerprint is:
`00894506db64e66c07c326c8909ac72426cc903df67ce9199768a7921702ac67`.

A key returned by the manifest response is never accepted as a trust anchor.
Unknown keys, unsigned/tampered manifests and unsafe trust files fail closed.
Custom Nexus environments need their independently verified public keys
provisioned by a trusted operator before setup. See the installation instructions
for the production default.

## Fresh installations only

Do not use this release to migrate an existing production installation or a
historical unsigned update journal. Existing installations remain operational
on their existing package. Migration, signing-key rotation, shared registry
credential revocation and per-site upgrades are separate operations.

Nexus must support opt-in signed previews and installed-release heartbeats
before this installer is offered. Production Nexus compatibility was deployed
first. Existing preview clients retain their original response shape.

## Installation

Create a separate installation in Nexus and follow [the setup instructions](README.md).
The guided flow retrieves authoritative identity and the approved manifest,
preserves management networking, and waits for the required browser password
change before finalizing. Resume an interruption with the same command.

The seven immutable assets include Tailscale 1.104.2. Verify separately downloaded
assets with `sha256sum -c SHA256SUMS`.

## Fresh directory preparation

Fresh setup handles restrictive host umasks when creating the ChirpStack backend
application-data directory. It preserves the safety checks on foreign, nonempty,
and linked directories. Installer 0.3.3 was stopped during private fresh-host
acceptance and was not publicly released.

## First-start scheduling and recovery

The installer waits reliably for the fast BACnet runtime initializer, including
when it has already exited successfully. Heartbeat scheduling continues when
the frontend certificate was not ready at the first attempt. Existing ownership,
image, mount, exit-code and credential checks remain enforced.

## Verification

- Ubuntu 26.04 CI: 1,259 tests, with three conditional checks skipped; 61
  installed-package smoke tests and four updater tests passed. Clean package
  build, extracted-package audit, Compose validation and bundle checks passed.
- Fresh enrollment reached the browser password gate directly; interruption and
  resume did not consume another enrollment key. The authenticated application
  password change and subsequent login succeeded. The temporary Nexus password
  became unavailable after the verified change.
- Automatic heartbeats reported release `0.2.11`, nine containers, CPU/memory/disk
  metrics and healthy VPN without a deployment check or a manual agent start.
- Controlled reboot restored all nine healthy services, all four enabled
  recurring timers, VPN, HTTPS on ports 443/5000, ChirpStack on 8080 and the
  Basic Station TCP listener on 3001. Application login remained valid.
- A stopped installer-owned backend recovered automatically to healthy in
  101 seconds. Independent source review found no defects.

The lab contained no gateway or sensor data. Existing on-site migrations are
outside this new-install release.

Debian package SHA-256:
`e1347b4f8adfc90d8caf95a20a7bcda38a2d613e923176f65b8b1787362085c3`.
