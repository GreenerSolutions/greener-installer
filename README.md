# Greener Building Server Installer

This public repository distributes the package-native Greener Building Server
installer for fresh **Ubuntu 24.04 amd64** hosts.

Only release documentation is stored in Git. Installer archives and checksum
files are published as versioned [GitHub Release assets](../../releases). This
repository does not contain Greener application source code, enrollment keys,
registry credentials, operator passwords, production configuration, or
customer data.

## Current pilot

The current release is a controlled pilot. Install it only on a fresh,
disposable Ubuntu 24.04 amd64 host. Docker Engine with the Docker Compose plugin
must already be installed and available as `/usr/bin/docker` with the local
socket at `/var/run/docker.sock`.

Download, verify, and unpack the pinned pilot release:

```bash
sudo apt update
sudo apt install -y ca-certificates curl

install -d -m 0700 "$HOME/greener-pilot"
cd "$HOME/greener-pilot"

base_url="https://github.com/GreenerSolutions/greener-installer/releases/download/v0.1.0-pilot.1"
archive="greener-pilot-kit-final-20261016.tar.gz"

curl --fail --location --proto '=https' --tlsv1.2 --remote-name "$base_url/$archive"
curl --fail --location --proto '=https' --tlsv1.2 --remote-name "$base_url/$archive.sha256"
sha256sum -c "$archive.sha256"
tar -xzf "$archive"
cd greener-pilot-kit-final-20261016

sudo ./check-host.sh
sha256sum -c SHA256SUMS
(cd release-bundle && sha256sum -c SHA256SUMS)
sudo apt install ./release-bundle/greener-building-server_*_amd64.deb
sudo greener status
less PILOT.md
```

The included `PILOT.md` is the authoritative runbook for configuration,
enrollment, activation, heartbeat enablement, reboot validation, updates, and
support evidence.

## Trust boundary

The archive is secret-free and intentionally contains invalid replacement
placeholders. Site configuration, the Nexus URL, the one-time enrollment key,
the registry host, and approved immutable image digests must be supplied through
the controlled installation process. Never post those values in this repository
or in a GitHub issue.

Published assets are versioned and must not be replaced in place. A corrected
build receives a new release tag.

See [SECURITY.md](SECURITY.md) before reporting a security issue.
