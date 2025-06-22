# Homeserver

This is a simple setup for running multiple self-hosted applications through Docker. Each application has its own sub-directory here.

Dependencies to install before trying to run this:

- [Docker](https://docs.docker.com/engine/install/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## How to run things

First create a `.env` for every sub-directory:

1. Duplicate the `.env.example` and rename that file to `.env`
2. Replace the dummy password in `.env` with a secure password of your choice
3. If applicable, replace the timezone (TZ) variable with [your timezone](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)

For each application that needs to be started up, run the following:

```bash
docker compose up -d
```

To shut down a given application, run the following in that application's sub-directory:

```bash
docker compose down
```

To update a container, run the following before restarting the container:

```bash
docker compose pull
# then shut down and restart the container for changes to take effect
```
