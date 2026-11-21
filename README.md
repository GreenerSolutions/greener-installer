# Greener Building Server Installer

This public repository distributes the package-native Greener Building Server
installer for fresh **Ubuntu 26.04 amd64** hosts.

Only release documentation is stored in Git. Installer archives and checksum
files are published as versioned [GitHub Release assets](../../releases). This
repository does not contain Greener application source code, enrollment keys,
registry credentials, operator passwords, production configuration, or
customer data.

## Current release

Use `v0.3.8` on a fresh **Ubuntu 26.04 amd64** machine. Recommended resources:
4 CPUs, 8 GiB RAM, and 80 GiB free storage.

This release is for **new installations** and a centrally coordinated installer
package-upgrade rollout. Existing deployed servers can keep running their
current installer; before signed package upgrades can be enabled, each existing
host needs a one-time coordinated package installation and provisioned release
trust.

### 1. Create the installation in Nexus

Open [Nexus](https://nexus.smarterbuildings.ca) → **Installations**, create the
building installation, and copy its **enrollment key**. The building, machine, BACnet device identity,
and approved application release come from this record.

If you lose the key before enrolling, choose **Replace key** on that installation.
The replacement invalidates the old key and preserves the installation details.
The temporary application password is a separate credential used later.

### 2. Run the installer on Ubuntu

```bash
curl --fail --location --proto '=https' --tlsv1.2 \
  https://github.com/GreenerSolutions/greener-installer/releases/download/v0.3.8/install-release-bundle.sh \
  -o install-release-bundle.sh &&
sudo bash ./install-release-bundle.sh --setup
```

The command installs missing prerequisites and the verified installer bundle,
then prompts for the enrollment key privately. Do not put the key in a command
or send it to anyone.

The installer detects each physical Ethernet port and asks whether to enable it.
For each enabled port, choose Keep, DHCP, or Static; Static requires you to enter
an address and prefix. Choose a purpose preset or type a custom label. Internet
routing and the BACnet/building interface are selected separately.

Review the summary before applying. Network changes use a 120-second trial.
Confirm within that window, including after reconnecting if SSH disconnects;
unconfirmed changes roll back automatically. Purpose labels do not change routing.
Live multi-interface disconnect and reboot acceptance for this release is deferred;
see the release notes before using the new network flow.

The installer retrieves the approved deployment manifest from Nexus, prepares the
runtime, downloads the application images, and starts the services. When prompted,
open the displayed application URL and change the temporary application password
shown in Nexus. Leave the terminal running so the installer can verify the change
and finish activation.

If interrupted, run the same command again. An installation already prepared on
the machine resumes from its saved checkpoint without asking for its identity or
consuming another key. You can also resume with:

```bash
sudo greener setup --guided
```

Explicit operator settings and existing local installations retain their configured
endpoints. Installing the Debian package alone does not activate containers.
See [release notes](RELEASE_NOTES_v0.3.8.md) for compatibility details.

New installs verify approved SmarterBuildings Core manifests with the production
public key included in the package. Normal heartbeats report the installed
application release to Nexus.


### Service addresses

The completed installer displays the addresses to open. Fresh guided setups make
the building application available over HTTPS on ports `443` and `5000`, and
ChirpStack on TCP `8080`, through the machine's IPv4 interfaces. Basic Station uses
TCP `3001`, BACnet/IP uses UDP `47808`, and OCPP uses TCP `9000` when selected.
Existing configurations keep their saved bind settings. Use the building LAN
or managed VPN to access these ports.

The production services are:

- Nexus: `https://nexus.smarterbuildings.ca`
- VPN: `https://connect.smarterbuildings.ca`
- Images: `registry.smarterbuildings.ca`

### Manual bundle installation

Each immutable release contains exactly these seven bootstrap assets:

- `greener-building-server_0.3.8-1_amd64.deb`
- `tailscale_1.104.2_amd64.deb`
- `tailscale-package.json`
- `install-release-bundle.sh`
- `installer-entrypoint.json`
- `release-manifest.json`
- `SHA256SUMS`

The GitHub release also includes the signed `installer-release.json` file for
Nexus package approval.

Manual bundle installation expects Docker Engine and its Compose plugin to be
installed and running. To install a bundle downloaded separately, verify it with
`sha256sum -c SHA256SUMS` and run `sudo bash ./install-release-bundle.sh "$PWD"` from its directory.
Run `sudo greener setup --guided` afterward to start or resume guided activation.
The installer validates the operating system, configuration, ownership, listeners,
immutable image digests, and enrollment state before activation.

## Trust boundary

The release bundle is secret-free and contains only the seven audited installer
assets listed above. A one-time enrollment key authorizes retrieval of the
installation identity and approved immutable manifest from Nexus. Signed
package-upgrade approval is separate metadata and is not a credential. Local
network settings are confirmed on the machine. Never post credentials, private
site configuration, or customer data in this repository or in a GitHub issue.

Published assets are versioned and must not be replaced in place. A corrected
build receives a new release tag.

See [SECURITY.md](SECURITY.md) before reporting a security issue.
