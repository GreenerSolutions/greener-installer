# Greener Building Server Installer

This public repository distributes the package-native Greener Building Server
installer for fresh **Ubuntu 26.04 amd64** hosts.

Only release documentation is stored in Git. Installer archives and checksum
files are published as versioned [GitHub Release assets](../../releases). This
repository does not contain Greener application source code, enrollment keys,
registry credentials, operator passwords, production configuration, or
customer data.

## Current release

Install `v0.1.0` only on a fresh Ubuntu 26.04 amd64 host. Docker Engine with the
Docker Compose plugin must already be installed and available as
`/usr/bin/docker` with the local socket at `/var/run/docker.sock`.

Download and verify every asset in the immutable release bundle:

```bash
sudo apt update
sudo apt install -y ca-certificates curl

install -d -m 0700 "$HOME/greener-installer-v0.1.0"
cd "$HOME/greener-installer-v0.1.0"

base_url="https://github.com/GreenerSolutions/greener-installer/releases/download/v0.1.0"
assets="
greener-building-server_0.1.0-64_amd64.deb
tailscale_1.104.2_amd64.deb
tailscale-package.json
install-release-bundle.sh
installer-entrypoint.json
release-manifest.json
SHA256SUMS
"

for asset in $assets; do
  curl --fail --location --proto '=https' --tlsv1.2 --remote-name "$base_url/$asset"
done

sha256sum -c SHA256SUMS
sudo ./install-release-bundle.sh "$PWD"
sudo greener status
```

The installer validates Ubuntu 26.04, configuration, ownership, listeners,
immutable image digests, and enrollment state before activation. Building VPN
enrollment is package-managed through Greener Nexus and uses short-lived,
single-use credentials; no reusable VPN key is included in the release.

After activation, the BACnet application is served over HTTPS on ports `443`
and `5000`. ChirpStack uses TCP `8080`, Basic Station uses TCP `3001`, and
BACnet/IP uses UDP `47808` when those modules are selected.

## Trust boundary

The release bundle is secret-free and contains only the seven audited installer
assets listed above. Site configuration, the Nexus URL, a one-time enrollment
key, the registry host, and approved immutable image digests must be supplied
through the controlled installation process. Never post those values in this
repository or in a GitHub issue.

Published assets are versioned and must not be replaced in place. A corrected
build receives a new release tag.

See [SECURITY.md](SECURITY.md) before reporting a security issue.
