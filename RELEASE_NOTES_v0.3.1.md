# Greener Installer v0.3.1

Package `greener-building-server_0.3.1-1_amd64.deb` supports fresh Ubuntu 26.04
amd64 machines with Docker Engine and the Docker Compose plugin.

This release adds the managed OCPP proxy as the ninth application service and
completes its activation, interrupted-startup recovery, update validation,
telemetry and restore handling. The default proxy destination follows the local
backend's configured bind address, including IPv6, and preflight rejects
overlapping required listeners.

The release also includes transactional application-configuration editing,
authenticated Nexus configuration jobs and bounded service settings. Existing
seven/eight-image update bases remain supported. Eight/nine-image backup and
restore preserve their generation-specific contracts; backup archives require
the matching installer version and custom data mounts need separate backups.

Fresh activation requires a compatible Nexus API, a one-time enrollment key
and a centrally approved **nine-image deployment manifest**, including
`ocpp_proxy`. Installing this package alone does not release or activate
application images. Enter the enrollment key at the guided terminal prompt.

Download all seven release assets and verify `SHA256SUMS`. The bundle includes
the exact tested Tailscale 1.104.2 amd64 package and the verified installation
entrypoint. Production defaults remain `nexus.smarterbuildings.ca`,
`connect.smarterbuildings.ca`, and `registry.smarterbuildings.ca`.

Validation completed the 1,206-test installer suite with three skips, a separate root-only
gateway-volume regression, four legacy updater tests, compilation and shell
syntax checks, independent review, a clean Ubuntu 26.04 package build,
extracted-package audit, bundle and pilot-kit checksums, and installed-package
container smoke. The two opt-in real-Docker groups were not rerun; the skipped
Debian metadata check passed separately with its required tool available.
Booted-machine activation, VPN/reboot recovery, hardware traffic and live remote
Apply/rollback remain installation acceptance checks.

The Debian package is 306,404 bytes, SHA-256:

```text
155542bea07e129310d5aa745a78fd5936b00b78ae13f13be94001264277dc76
```

Follow the current README for download, verification, guided enrollment,
activation and first-login password replacement. Never use the retired
`bootstrap.sh` workflow for a new installation.
