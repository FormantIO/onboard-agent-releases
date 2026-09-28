# onboard-agent releases

Public release binaries and installer for **onboard-agent**, the process that
runs on a Linux robot, dials out to the Formant Cortex relay and serves the
"Robot onboarding" agent's tools (discover, preview, propose, apply, ...) for
that machine. No SSH, no inbound ports.

This repo holds releases only; the source is private. Each release carries:

- `install.sh` — installer / upgrader / uninstaller (systemd unit, run as root)
- `onboard-agent-linux-x64`, `onboard-agent-linux-arm64` — standalone binaries
- `SHA256SUMS` — `sha256sum -c` compatible checksums of the files above

## Install

Get a source token from the Cortex console (Robots → Add robot), then on the robot:

```bash
curl -fsSL https://github.com/FormantIO/onboard-agent-releases/releases/latest/download/install.sh | sudo bash -s -- \
  --relay wss://mcp.metaphysics-dev.formant.ai/sources/ws --token <sourceToken> [--name <robot>]
```

The installer downloads the binary for the robot's architecture from the same
release and refuses it unless it matches that release's
[`SHA256SUMS`](https://github.com/FormantIO/onboard-agent-releases/releases/latest/download/SHA256SUMS).
`--version vX.Y.Z` pins a release, `--uninstall` removes the agent, and
`journalctl -u onboard-agent -f` shows its logs.

To verify by hand:

```bash
curl -fsSLO https://github.com/FormantIO/onboard-agent-releases/releases/latest/download/SHA256SUMS
curl -fsSLO https://github.com/FormantIO/onboard-agent-releases/releases/latest/download/onboard-agent-linux-x64
sha256sum -c --ignore-missing SHA256SUMS
```

All releases: https://github.com/FormantIO/onboard-agent-releases/releases
