# Apricaut House (Home Assistant add-on)

Connects your home to Apricaut Games: it plays effect cues (night, murder, verdict moods) on
your house through Home Assistant. The add-on holds one outbound connection to the games server
and applies each effect to your lights and plugs.

## Install

1. In Home Assistant, open **Settings → Add-ons → Add-on Store**, then the three-dot menu →
   **Repositories**, and add `https://github.com/apricaut/traitors`.
2. Install **Apricaut House** from the store. It installs a prebuilt image, so there is nothing
   to compile on your device.
3. On the add-on's **Configuration** tab, paste your **bridge token** into `BRIDGE_TOKEN`.
   Mint the token on the games site under **Account → Your house**; it is shown once, so copy
   it immediately. Leave `GAMES_URL` and `HA_URL` at their defaults unless told otherwise.
4. Start the add-on. It connects out to the games server and reports itself as your house's
   effect bridge. `HA_URL` defaults to the supervisor proxy, and the add-on uses the
   supervisor token, so no long-lived Home Assistant token is needed.

## Options

| Option         | What it is                                                        |
| -------------- | ----------------------------------------------------------------- |
| `GAMES_URL`    | The games server's bridge socket (a `wss://…/ws/bridge` URL).     |
| `BRIDGE_TOKEN` | The token minted on your house settings page. Shown once.         |
| `HA_URL`       | The Home Assistant base URL (defaults to the supervisor proxy).   |
| `ROLE`         | `bridge` (the effect bridge) or `display` (reserved).             |
| `HEALTH_PORT`  | The port the add-on serves `GET /healthz` on.                     |

## Build

The published add-on runs a prebuilt image (`image:` in `config.yaml`). CI cross-compiles the
bridge for each architecture and pushes `ghcr.io/apricaut/apricaut-house-{arch}` on every push
to `main` that touches the bridge (see `.github/workflows/publish-house.yml`). The Dockerfile
here is runtime-only: it copies the prebuilt binary onto the arch's Home Assistant base image
(`build.yaml`) and runs it.
