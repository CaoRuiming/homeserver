# OpenClaw

The Docker image and container is created, updated, and managed by a separately cloned https://github.com/openclaw/openclaw

This directory exists as a reference and a place to save OpenClaw config and workspace directories.

Documentation: https://docs.openclaw.ai/install/docker#docker

```sh
# Run from OpenClaw repo

# Custom Docker image and container config
# Set these variables before running docker_setup.sh
export OPENCLAW_CONFIG_DIR="~/Projects/homeserver/openclaw/data/.openclaw" && \
export OPENCLAW_WORKSPACE_DIR="~/Projects/homeserver/openclaw/data/.openclaw/workspace" && \
export OPENCLAW_EXTRA_MOUNTS="~/Bot:/home/node/shared:rw" && \
export OPENCLAW_HOME_VOLUME="openclaw_home"

# Internal commands from docker_setup.sh
docker build -t openclaw:local -f Dockerfile .
docker compose run --rm openclaw-cli onboard
docker compose up -d openclaw-gateway

# Starting up the container with configured overrides
docker compose -f ./docker-compose.yml -f ./docker-compose.extra.yml up openclaw-gateway
```

Secure auth for the web dashboard/chat interface is pain to get working when running OpenClaw within a container. To temporarily disable that strict auth (still requires tokens though), set `gateway.controlUI.allowInsecureAuth` to `true` in `openclaw.json`.