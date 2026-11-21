# Greener installer v0.3.8

Package: `greener-building-server_0.3.8-1_amd64.deb` for Ubuntu 26.04 amd64.

## Installer upgrades

The upgrade worker now follows GitHub's release redirects and verifies the
downloaded package against the Nexus-approved signature and SHA-256 digest.
Use this version for the one-time in-place upgrade on existing hosts.
Hosts already running `0.3.7-1` also need this one-time upgrade.

Provision the trusted installer-package public policy during that rollout.
Afterward, approve and schedule subsequent packages in
**Nexus → Installations → Installer upgrades**. The release includes a signed
`installer-release.json` for package approval.

This release includes the managed OCPP proxy and ChirpStack backend settings
and per-interface Ethernet setup described in the
[v0.3.7 release notes](RELEASE_NOTES_v0.3.7.md).

## Verification

The worker followed GitHub's real release redirect and verified the
production-signed package. The release bundle passed checksum validation.
Live building-host upgrades are deferred.

Debian package SHA-256: `c95dca2a413cfca4ac84b17b48e83105450a4663403a14fe3331725793f145c1`.

Source: [GreenerSolutions/install PR #26](https://github.com/GreenerSolutions/install/pull/26).
Build source commit: `64d85b25f54cfb35696f40ac5b94fbbdaf2e0c3e`.
