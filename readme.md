# strategos

NixOS configuration for **strategos** — a Raspberry Pi 3B+ (`aarch64-linux`) on the home LAN.
It currently runs as a small always-on box: SSH access (key-based, user `athena`) and a software
access point (`create_ap` on `wlan0`), behind the NixOS firewall.

The flake pins `nixpkgs` on the stable `release-25.11` channel and shares the
[`nixos-modules`](https://github.com/TheWhale01/nixos-modules) input with the rest of the fleet.

## The rest of the fleet

This repo used to hold all my NixOS configs; that era is over:

- **erebos** (homelab server) — has its own repo now: <https://github.com/TheWhale01/erebos>
- **pontos** (laptop) — migrated from NixOS to Fedora; no NixOS config to maintain anymore
- **olympos** — never built; was a planned gaming console with a config similar to pontos, and will
  probably never exist

## Install / update

Enable flakes first (if not already):

```nix
# /etc/nixos/configuration.nix (or the installer's config)
{
	nix.settings.experimental-features = [
		"nix-command"
		"flakes"
	];
}
```

Then, from a checkout of this repo:

```bash
# First install (from a NixOS installer on the Pi)
sudo nixos-install --flake .#strategos

# Routine switch on the running machine
sudo nixos-rebuild switch --flake .#strategos
```

## Layout

```
sys/
├── configuration.nix        # main module (hostname, users, firewall, networking)
├── hardware-configuration.nix
├── packages.nix
├── home.nix                 # home-manager for athena
├── home-manager/
└── services/
    ├── ssh.nix              # key-based SSH
    └── create_ap.nix        # software access point on wlan0
```

## Planned

- Keyring host: agenix identity-key backup target for erebos (see the `blog-ideas` vault,
  `todo/age-keys-to-strategos.md`) — passphrase-encrypted archive for now; the Pi has no agenix yet.
