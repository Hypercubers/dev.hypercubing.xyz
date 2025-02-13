# Server Setup

As of the end of 2024, there is a [DigitalOcean](https://www.digitalocean.com/) droplet dedicated to the Hypercubing server with the following specs:

- RAM: 2 GB
- Disk: 50 GB
- Region: SFO3
- OS: Ubuntu 24.04 (LTS) x64

In the event that this droplet is no longer running, contact HactarCE at <hello@ajfarkas.dev>.

This page contains instructions on how to recreate the server in case it is lost. All of these commands should be run on the server.

## Nextcloud

The server runs an instance of [Nextcloud](https://nextcloud.com/) which hosts the images for [hypercubing.xyz](https://hypercubing.xyz/) and [dev.hypercubing.xyz](https://dev.hypercubing.xyz/).

### Install Docker

```sh
curl -fsSL https://get.docker.com | sudo sh
```

### Create folder for Nextcloud

```sh
mkdir ~/nextcloud
```

### Configure Caddy

Caddy is used as a reverse proxy so that we can run multiple services on the same server accessible at different domains.

Save this file as `~/nextcloud/Caddyfile`:

```Caddyfile title="~/nextcloud/Caddyfile"
https://assets.hypercubing.xyz:443 {
    handle_path /* {
        root * /srv/assets
        file_server browse
    }
}

https://cloud.hypercubing.xyz:443 {
    handle_path /assets/* {
        root * /srv/assets
        file_server browse
    }
    handle {
        reverse_proxy * localhost:11000
    }
}

https://aio.cloud.hypercubing.xyz:443 {
    reverse_proxy https://localhost:8080 {
        transport http {
            tls_insecure_skip_verify
        }
    }
}

https://lb.hypercubing.xyz:443 {
    reverse_proxy localhost:3000
}
```

### Configure Docker

NextCloud and Caddy are run within Docker. We use Docker Compose to run multiple containers easily.

Save this file as `~/nextcloud/compose.yaml`. You may need to change `HactarCE/files/assets` to some other path; whatever path it is determines the files that will be publicly accessible at [assets.hypercubing.xyz](https://assets.hypercubing.xyz/).

```yaml title="~/nextcloud/compose.yaml"
services:
  nextcloud-aio-mastercontainer:
    image: docker.io/nextcloud/all-in-one:latest
    init: true
    restart: always
    container_name: nextcloud-aio-mastercontainer # This line is not allowed to be changed as otherwise AIO will not work correctly
    volumes:
      - nextcloud_aio_mastercontainer:/mnt/docker-aio-config # This line is not allowed to be changed as otherwise the built-in backup solution will not work
      - /var/run/docker.sock:/var/run/docker.sock:ro # May be changed on macOS, Windows or docker rootless. See the applicable documentation. If adjusting, don't forget to also set 'WATCHTOWER_DOCKER_SOCKET_PATH'!
    ports:
      # - 80:80 # Can be removed when running behind a web server or reverse proxy (like Apache, Nginx, Cloudflare Tunnel and else). See https://github.com/nextcloud/all-in-one/blob/main/reverse-proxy.md
      - 8080:8080
      # - 8443:8443 # Can be removed when running behind a web server or reverse proxy (like Apache, Nginx, Cloudflare Tunnel and else). See https://github.com/nextcloud/all-in-one/blob/main/reverse-proxy.md
    environment: # Is needed when using any of the options below
      # - AIO_DISABLE_BACKUP_SECTION=false # Setting this to true allows to hide the backup section in the AIO interface. See https://github.com/nextcloud/all-in-one#how-to-disable-the-backup-section
      - APACHE_PORT=11000 # Is needed when running behind a web server or reverse proxy (like Apache, Nginx, Cloudflare Tunnel and else). See https://github.com/nextcloud/all-in-one/blob/main/reverse-proxy.md
      - APACHE_IP_BINDING=127.0.0.1 # Should be set when running behind a web server or reverse proxy (like Apache, Nginx, Cloudflare Tunnel and else) that is running on the same host. See https://github.com/nextcloud/all-in-one/blob/main/reverse-proxy.md
      # - BORG_RETENTION_POLICY=--keep-within=7d --keep-weekly=4 --keep-monthly=6 # Allows to adjust borgs retention policy. See https://github.com/nextcloud/all-in-one#how-to-adjust-borgs-retention-policy
      # - COLLABORA_SECCOMP_DISABLED=false # Setting this to true allows to disable Collabora's Seccomp feature. See https://github.com/nextcloud/all-in-one#how-to-disable-collaboras-seccomp-feature
      - NEXTCLOUD_DATADIR=/mnt/ncdata # Allows to set the host directory for Nextcloud's datadir. ⚠️⚠️⚠️ Warning: do not set or adjust this value after the initial Nextcloud installation is done! See https://github.com/nextcloud/all-in-one#how-to-change-the-default-location-of-nextclouds-datadir
      # - NEXTCLOUD_MOUNT=/mnt/ # Allows the Nextcloud container to access the chosen directory on the host. See https://github.com/nextcloud/all-in-one#how-to-allow-the-nextcloud-container-to-access-directories-on-the-host
      # - NEXTCLOUD_UPLOAD_LIMIT=10G # Can be adjusted if you need more. See https://github.com/nextcloud/all-in-one#how-to-adjust-the-upload-limit-for-nextcloud
      # - NEXTCLOUD_MAX_TIME=3600 # Can be adjusted if you need more. See https://github.com/nextcloud/all-in-one#how-to-adjust-the-max-execution-time-for-nextcloud
      # - NEXTCLOUD_MEMORY_LIMIT=512M # Can be adjusted if you need more. See https://github.com/nextcloud/all-in-one#how-to-adjust-the-php-memory-limit-for-nextcloud
      # - NEXTCLOUD_TRUSTED_CACERTS_DIR=/path/to/my/cacerts # CA certificates in this directory will be trusted by the OS of the nexcloud container (Useful e.g. for LDAPS) See See https://github.com/nextcloud/all-in-one#how-to-trust-user-defined-certification-authorities-ca
      # - NEXTCLOUD_STARTUP_APPS=deck twofactor_totp tasks calendar contacts notes # Allows to modify the Nextcloud apps that are installed on starting AIO the first time. See https://github.com/nextcloud/all-in-one#how-to-change-the-nextcloud-apps-that-are-installed-on-the-first-startup
      - NEXTCLOUD_STARTUP_APPS=twofactor_totp notes # Allows to modify the Nextcloud apps that are installed on starting AIO the first time. See https://github.com/nextcloud/all-in-one#how-to-change-the-nextcloud-apps-that-are-installed-on-the-first-startup
      # - NEXTCLOUD_ADDITIONAL_APKS=imagemagick # This allows to add additional packages to the Nextcloud container permanently. Default is imagemagick but can be overwritten by modifying this value. See https://github.com/nextcloud/all-in-one#how-to-add-os-packages-permanently-to-the-nextcloud-container
      # - NEXTCLOUD_ADDITIONAL_PHP_EXTENSIONS=imagick # This allows to add additional php extensions to the Nextcloud container permanently. Default is imagick but can be overwritten by modifying this value. See https://github.com/nextcloud/all-in-one#how-to-add-php-extensions-permanently-to-the-nextcloud-container
      # - TALK_PORT=3478 # This allows to adjust the port that the talk container is using. See https://github.com/nextcloud/all-in-one#how-to-adjust-the-talk-port
      # - WATCHTOWER_DOCKER_SOCKET_PATH=/var/run/docker.sock # Needs to be specified if the docker socket on the host is not located in the default '/var/run/docker.sock'. Otherwise mastercontainer updates will fail. For macos it needs to be '/var/run/docker.sock'
    # # Uncomment the following line when using SELinux
    # security_opt: ["label:disable"]

  # Optional: Caddy reverse proxy. See https://github.com/nextcloud/all-in-one/blob/main/reverse-proxy.md
  # You can find further examples here: https://github.com/nextcloud/all-in-one/discussions/588
  caddy:
    image: docker.io/caddy:alpine
    restart: always
    container_name: caddy
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - ./certs:/certs
      - ./config:/config
      - ./data:/data
      - ./sites:/srv
      - /mnt/ncdata/HactarCE/files/assets:/srv/assets
    network_mode: "host"

volumes: # If you want to store the data on a different drive, see https://github.com/nextcloud/all-in-one#how-to-store-the-filesinstallation-on-a-separate-drive
  nextcloud_aio_mastercontainer:
    name: nextcloud_aio_mastercontainer # This line is not allowed to be changed as otherwise the built-in backup solution will not work
```

### Start Caddy + Nextcloud

```sh
cd ~/nextcloud
docker compose up -d
systemctl enable --now docker
```

### Ensure the domain records point to the server

Ensure these domain records exist for `hypercubing.xyz`, all set to the IP address of the server:

| Type | Name            |
| ---- | --------------- |
| A    | aio.cloud       |
| A    | assets          |
| A    | cloud           |

And these records for GitHub pages:

| Type  | Name            | Content               |
| ----- | --------------- | --------------------- |
| A     | hypercubing.xyz | 185.199.108.153       |
| A     | hypercubing.xyz | 185.199.109.153       |
| A     | hypercubing.xyz | 185.199.110.153       |
| A     | hypercubing.xyz | 185.199.111.153       |
| CNAME | www             | hypercubers.github.io |

And these records for other subdomains:

| Type  | Name    | Content                       |
| ----- | ------- | ----------------------------- |
| CNAME | archive | hypercubing-archive.gitlab.io |
| CNAME | dev     | hypercubers.github.io         |

If using Cloudflare, proxy status should be set to "DNS only" for all of these records.

### Initialize Nextcloud

Go to <https://aio.cloud.hypercubing.xyz/>. Note the passphrase somewhere safe, then click **Open Nextcloud AIO login**.

If you have access to a backup, save it on the server in `/mnt/backup` and enter that for **Local backup location**. Leave **Remote borg repo** blank. Enter the **Borg passphrase** saved from when the backup was created.

## Leaderboards

The leaderboards are hosted by a single Rust program that controls the Discord bot, database, and web server.

1. Follow the database setup and running instructions in `README.md` at `https://github.com/Hypercubers/hsc-leaderboard`.
2. Run `crontab -e` and add the following line:

```cron
@reboot sh -c "cd ~/hsc-leaderboard/ && ./hsc-leaderboard"
```

## Fancy command line setup

```sh
# global
sudo apt install make bat btm cloc eza fd-find fish fzf gh hexyl jq ripgrep sd trash-cli unp zoxide
sudo snap install zellij --classic
curl -sS https://starship.rs/install.sh | sh
```

```fish
# per user, in fish
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source .cargo/env.fish
cargo install du-dust pfetch
curl -sL https://raw.githubusercontent.com/jorgebucaran/fisher/main/functions/fisher.fish | source
fisher install jorgebucaran/fisher
fisher install PatrickF1/fzf.fish
fisher install lewisacidic/fish-git-abbr
chsh -s /usr/bin/fish
```

On local machine with fish shell config:

```sh
scp -r .config/fish/{config.fish,fish_plugins} <user>@hypercubing:.config/fish/
```
