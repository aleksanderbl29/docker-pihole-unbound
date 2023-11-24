# Pi-Hole + Unbound on Docker

[![Build and publish container](https://github.com/aleksanderbl29/docker-pihole-unbound/actions/workflows/auto-build-container.yml/badge.svg)](https://github.com/aleksanderbl29/docker-pihole-unbound/actions/workflows/auto-build-container.yml)

## Use Docker to run [Pi-Hole](https://pi-hole.net) with an upstream [Unbound](https://nlnetlabs.nl/projects/unbound/about/) resolver

- Install Unbound directly into the Pi-Hole container
  - This configuration contacts the DNS root servers directly, please read the Pi-Hole docs on [Pi-hole as All-Around DNS Solution](https://docs.pi-hole.net/guides/unbound/) to understand what this means.
  - With this approach, we can also simplify our Docker networking since `macvlan` is no longer necessary.

## Updates

This image is regularly update with the latest release from the official pi-hole image.

Whenever there is an update for the [original pihole image](https://hub.docker.com/r/pihole/pihole) a Github Action publishes an update for this image with the new image.

The workflow file for this can be found in `.github/workflows/auto-build-container.yml`

This workflow runs when the image tag is updated in `pihole-unbound/Dockerfile`. I do that semi-manually with the help of [the renovate bot](https://github.com/renovatebot/renovate). Therefore you may experience some delay whenever there is an update for the [original pihole image](https://hub.docker.com/r/pihole/pihole)

## Description

This Docker deployment runs both Pi-Hole and Unbound in a single container.

The base image for the container is the [official Pi-Hole container](https://hub.docker.com/r/pihole/pihole), with an extra build step added to install the Unbound resolver directly into to the container based on [instructions provided directly by the Pi-Hole team](https://docs.pi-hole.net/guides/unbound/).

## Usage

First create a `.env` file to substitute variables for your deployment.

### Pi-hole environment variables

> Vars and descriptions replicated from the [official pihole container](https://github.com/pi-hole/docker-pi-hole/#environment-variables):

| Variable | Default | Value | Description |
| -------- | ------- | ----- | ---------- |
| `TZ` | UTC | `<Timezone>` | Set your [timezone](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) to make sure logs rotate at local midnight instead of at UTC midnight.
| `WEBPASSWORD` | random | `<Admin password>` | [http://pi.hole/admin](http://pi.hole/admin) password. Run `docker logs pihole \| grep random` to find your random pass.
| `FTLCONF_LOCAL_IPV4` | unset | `<Host's IP>` | Set to your server's LAN IP, used by web block modes and lighttpd bind address.
| `REV_SERVER` | `false` | `<"true"\|"false">` | Enable DNS conditional forwarding for device name resolution |
| `REV_SERVER_DOMAIN` | unset | Network Domain | If conditional forwarding is enabled, set the domain of the local network router |
| `REV_SERVER_TARGET` | unset | Router's IP | If conditional forwarding is enabled, set the IP of the local network router |
| `REV_SERVER_CIDR` | unset | Reverse DNS | If conditional forwarding is enabled, set the reverse DNS zone (e.g. `192.168.0.0/24`) |
| `WEBTHEME` | `default-light` | `<"default-dark"\|"default-darker"\|"default-light"\|"default-auto"\|"lcars">`| User interface theme to use. |
| `VIRTUAL_HOST` | `$FTLCONF_LOCAL_IPV4` | `<Custom Hostname>` | What your web server 'virtual host' is, accessing admin through this Hostname/IP allows you to make changes to the whitelist / blacklists in addition to the default ['http://pi.hole/admin'](http://pi.hole/admin) address |

Example `.env` file in the same directory as your `docker-compose.yaml` file:

```env
FTLCONF_LOCAL_IPV4=192.168.1.10
TZ=America/Los_Angeles
WEBPASSWORD=QWERTY123456asdfASDF
REV_SERVER=true
REV_SERVER_DOMAIN=local
REV_SERVER_TARGET=192.168.1.1
REV_SERVER_CIDR=192.168.0.0/16
HOSTNAME=pihole
DOMAIN_NAME=pihole.local
PIHOLE_WEBPORT=80
WEBTHEME=default-light
VIRTUAL_HOST=pihole.box
```

### Running the stack

```bash
docker-compose up -d
```

> If using Portainer, just paste the `docker-compose.yaml` contents into the stack config and add your *environment variables* directly in the UI.

## Building the image locally

- [ ] Clone this repo to you machine
- [ ] Run the commands below

```bash
cd docker-pihole-unbound
docker build . -t dev/docker-pihole-unbound:latest
```

## Automatic dev builds with Github Actions

I have setup a Github Aciton that runs on all pull requests that builds and publishes the image configured in the repo. The action can be found in `.github/workflows/dev-build.yml`. To use this feature please comment your repo and tag me (@aleksanderbl29) and I will run the workflow for you.

## Contributors

Thank you to all contributors for help making this project better.

<!-- readme: contributors -start -->
<table>
<tr>
    <td align="center">
        <a href="https://github.com/aleksanderbl29">
            <img src="https://avatars.githubusercontent.com/u/73799306?v=4" width="100;" alt="aleksanderbl29"/>
            <br />
            <sub><b>Aleksander Bang-Larsen</b></sub>
        </a>
    </td>
    <td align="center">
        <a href="https://github.com/chriscrowe">
            <img src="https://avatars.githubusercontent.com/u/425808?v=4" width="100;" alt="chriscrowe"/>
            <br />
            <sub><b>Chris Crowe</b></sub>
        </a>
    </td>
    <td align="center">
        <a href="https://github.com/ChevySSinSD">
            <img src="https://avatars.githubusercontent.com/u/47265616?v=4" width="100;" alt="ChevySSinSD"/>
            <br />
            <sub><b>Null</b></sub>
        </a>
    </td>
    <td align="center">
        <a href="https://github.com/xaabi6">
            <img src="https://avatars.githubusercontent.com/u/22637294?v=4" width="100;" alt="xaabi6"/>
            <br />
            <sub><b>Xabi</b></sub>
        </a>
    </td>
    <td align="center">
        <a href="https://github.com/chronicblondiee">
            <img src="https://avatars.githubusercontent.com/u/17150999?v=4" width="100;" alt="chronicblondiee"/>
            <br />
            <sub><b>Chronicblondiee</b></sub>
        </a>
    </td>
    <td align="center">
        <a href="https://github.com/PromoFaux">
            <img src="https://avatars.githubusercontent.com/u/1998970?v=4" width="100;" alt="PromoFaux"/>
            <br />
            <sub><b>Adam Warner</b></sub>
        </a>
    </td></tr>
<tr>
    <td align="center">
        <a href="https://github.com/dszymczuk">
            <img src="https://avatars.githubusercontent.com/u/539352?v=4" width="100;" alt="dszymczuk"/>
            <br />
            <sub><b>Damian Szymczuk</b></sub>
        </a>
    </td>
    <td align="center">
        <a href="https://github.com/gramsaran">
            <img src="https://avatars.githubusercontent.com/u/356823?v=4" width="100;" alt="gramsaran"/>
            <br />
            <sub><b>Greg Ramsaran</b></sub>
        </a>
    </td>
    <td align="center">
        <a href="https://github.com/Sinnohd">
            <img src="https://avatars.githubusercontent.com/u/2531453?v=4" width="100;" alt="Sinnohd"/>
            <br />
            <sub><b>Jens Brey</b></sub>
        </a>
    </td>
    <td align="center">
        <a href="https://github.com/LawnMo">
            <img src="https://avatars.githubusercontent.com/u/81721212?v=4" width="100;" alt="LawnMo"/>
            <br />
            <sub><b>Null</b></sub>
        </a>
    </td>
    <td align="center">
        <a href="https://github.com/MaHl111">
            <img src="https://avatars.githubusercontent.com/u/48538049?v=4" width="100;" alt="MaHl111"/>
            <br />
            <sub><b>Null</b></sub>
        </a>
    </td>
    <td align="center">
        <a href="https://github.com/TheOftedal">
            <img src="https://avatars.githubusercontent.com/u/78422637?v=4" width="100;" alt="TheOftedal"/>
            <br />
            <sub><b>Mats Oftedal</b></sub>
        </a>
    </td></tr>
<tr>
    <td align="center">
        <a href="https://github.com/ZSamuels28">
            <img src="https://avatars.githubusercontent.com/u/8294014?v=4" width="100;" alt="ZSamuels28"/>
            <br />
            <sub><b>Null</b></sub>
        </a>
    </td></tr>
</table>
<!-- readme: contributors -end -->
