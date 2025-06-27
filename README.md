# Homeserver

This is a simple setup for running multiple self-hosted applications through Docker. Each application has its own sub-directory here.

Dependencies to install before trying to run this:

- [Docker](https://docs.docker.com/engine/install/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## How to run each application

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

## Homeserver system setup reference

Various resources and snippets I found helpful when setting up a mini desktop running [Pop!\_OS](https://system76.com/pop/) (essentialy Ubuntu) as a homeserver.

### Generally Useful Software and Utilities

- Firefox Dev Edition (through [Ubuntu Make](https://wiki.ubuntu.com/ubuntu-make))
- [Docker Engine](https://docs.docker.com/engine/install/ubuntu/)
- [yt-dlp](https://github.com/yt-dlp/yt-dlp)
- [various codecs](https://support.system76.com/articles/codecs/)
- Media
  - [Handbrake](https://handbrake.fr/)
  - ffmpeg
  - VLC
- [Ungoogled Chromium](https://github.com/ungoogled-software/ungoogled-chromium)

#### Unattended Upgrades

```sh
sudo apt install unattended-upgrades
sudo dpkg-reconfigure unattended-upgrades
```

Config file located in `/etc/apt/apt.conf.d/50unattended-upgrades`,

### Automatically Mounting Extra Drives

Reference: https://support.system76.com/articles/extra-drive/

### Configuring LAN, Local DNS, and SSL

Assign the homeserver a static LAN IP through router/DHCP server.

Use a local DNS server like PiHole and create an local DNS record routing `*.home.arpa` (e.g. `homeserver.home.arpa`) domains to the static IP address for plain HTTP access.

For configuring HTTPS and SSL stuff, configure an A Record for an owned domain (e.g. `home.mydomain.dev`) and configure a wildcard CNAME record (e.g. `*.home.mydomain.dev`) pointing to that A Record's domain (`home.mydomain.dev`).

To let the DNS verification go through (at least for Namecheap domains), an additional `TXT` record may need to be created with host set to `@` and value set to `v=spf1 include:spf.efwd.registrar-servers.com ~all`.

See the following video as a helpful reference for setting up SSL locally: https://www.youtube.com/watch?v=qlcVx-k-02E

Once this is set up, a reverse proxy (e.g. Nginx Proxy Manager) can be configured to route traffic from things like `https://someapp.home.mydomain.dev` to the container running the corresponding application.

### NFS Setup

Reference: https://ubuntu.com/server/docs/network-file-system-nfs

To install:

```sh
sudo apt install nfs-kernel-server
```

To start the service:

```sh
sudo systemctl start nfs-kernel-server.service
```

Config (`/etc/exports`):

```
# /etc/exports: the access control list for filesystems which may be exported
#		to NFS clients.  See exports(5).
#
# Example for NFSv2 and NFSv3:
# /srv/homes       hostname1(rw,sync,no_subtree_check) hostname2(ro,sync,no_subtree_check)
#
# Example for NFSv4:
# /srv/nfs4        gss/krb5i(rw,sync,fsid=0,crossmnt,no_subtree_check)
# /srv/nfs4/homes  gss/krb5i(rw,sync,no_subtree_check)
#

/home/myusername/Storage 10.0.0.0/16(rw,sync,no_subtree_check,all_squash,anonuid=1000,anongid=1000,insecure)
```

URL to mount the drive: `nfs://homeserver.home.arpa/home/myusername/Storage`

### Further Reading

https://www.pcmag.com/how-to/copy-your-windows-installation-to-an-ssd

https://support.system76.com/articles/switch-from-macos-to-popos/
