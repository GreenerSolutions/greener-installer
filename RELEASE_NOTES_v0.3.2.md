# Greener installer v0.3.2

Debian package: `greener-building-server_0.3.2-1_amd64.deb`.

## Guided installation

Create an installation in Nexus, then run the command in the
[installation instructions](README.md). The bootstrap entrypoint installs the
verified seven-asset bundle and missing prerequisites, then opens guided setup.
The enrollment key is entered privately in the terminal.

Nexus supplies the building and machine identity, BACnet device identity,
registry host, and compatible approved deployment manifest. These fields are
no longer requested again on the machine. Previewing this information does not
consume the enrollment key. A missing explicitly supplied manifest is rejected
before asking for a key.

The installer asks for local settings, including the building Ethernet port and
whether to use existing Ubuntu networking or configure a static building port.
Static setup validates the selected interface and uses Ubuntu's timed network
confirmation. Existing management networking must remain available. Unsupported
or ambiguous network configurations require configuration in Ubuntu before
choosing the existing-networking option.

Guided setup prepares, pulls, and activates the runtime. It displays application
addresses at the required password-change checkpoint and completes activation
after verifying the change. An interrupted run resumes with the same command.
A completed run reports its existing state instead of starting another setup.

Fresh guided setups expose ChirpStack on the machine's IPv4 interfaces and show
its port 8080 address. Existing saved connectivity settings retain their values.

## Nexus key replacement

Before enrollment, an issued installation can use **Replace key** in Nexus if its
one-time key was lost. The new key invalidates the old verifier while preserving
the installation identity, BACnet settings, expiry, and temporary application
credentials. In-use, completed, revoked, or expired enrollments cannot rotate
through this action.

## Compatibility

Existing explicit validation, staging, preparation, and activation commands remain
available. Package installation alone does not activate containers. Upgrading a
completed machine preserves its configuration and running application services.

This release retains the Docker 29 ChirpStack helper-metadata compatibility fix
from v0.3.1-2. Application release `0.2.11` and its immutable image references are
unchanged. The new installer requires the corresponding Nexus preview API for a
fresh simplified setup; existing prepared transactions resume from local state.

Published release assets are immutable. Every bundle contains the Debian package,
the pinned Tailscale package, their metadata, the bootstrap entrypoint, release
metadata, and `SHA256SUMS`. Verify a separately downloaded bundle with
`sha256sum -c SHA256SUMS`.
