# Greener Building Server v0.3.1-2

Debian package `greener-building-server_0.3.1-2_amd64.deb` targets Ubuntu 26.04
amd64. Installer/API version remains `0.3.1`; this is a package revision of the
existing installer, with no application-image or deployment-manifest change.

Docker 29.1.3 includes an empty `DriverConfig` object in the ChirpStack key-init
helper's named-volume metadata. The prior exact-shape check rejected this after
the helper completed successfully, leaving activation at `chirpstack_key_creating`.
The correction accepts that empty default and preserves every named-volume,
no-copy, read-only, local-driver and other helper validation check. Nonempty
driver configuration and unknown fields remain rejected.

## Upgrade a paused installation

Keep its existing journal, helper, volumes and recovery data. Download and verify
the new Debian package against the published `SHA256SUMS`, then install it:

```bash
sudo dpkg -i ./greener-building-server_0.3.1-2_amd64.deb
sudo greener activate --resume
```

Activation reuses the completed helper and existing generated key. Do not rerun
guided setup to start a second enrollment. A successful resume proceeds to the
normal first-login password-change checkpoint; booted-host and hardware testing
remain manual acceptance work.

## Validation

The 33-test ChirpStack bootstrap module and independent review passed. Full
[Ubuntu 26 CI](https://github.com/GreenerSolutions/install/actions/runs/42755529609)
passed its 1,206-test suite, four legacy updater tests, package build/audit and
61 installed-package regression tests. The release bundle's six payload
checksums passed; the extracted release package contains the tested guard.

Package SHA-256:
`9c7ccec978f9ff17bb8fc8822a2658c9d818eaddb7f7a7d6959a0d568dfa25dd`.

This immutable release contains the same seven asset types as v0.3.1. Its assets
are published under the new tag; the previous release remains unchanged.
