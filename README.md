# Rygel DLNA/uPNP server docker image

Dockerfile to build a Rygel DLNA/uPNP server docker image. Forked from [sameersbn/docker-rygel](https://github.com/sameersbn/docker-rygel), but since he uses his
own ubuntu docker image, I wanted this to be more straightforward.

There are many DLNA/UPnP devices on the market, such as the Sony Playstation 3, smartphones, Microsoft XBox 360, DLNA speakers, and TVs from Sony and Samsung, with more expected. ([project homepage](https://wiki.gnome.org/Projects/Rygel/))

**Table of Contents**
- [Installation](#installation)
- [Quick Start](#quick-start)
    - [Cache store](#cache-store)
- [Shell Access](#shell-access)
- [Upgrading](#upgrading)
- [Docker Compose](#docker-compose)

## Installation

Automated builds of the image are available on [Dockerhub](https://hub.docker.com/r/tomsotte/rygel) and is the recommended method of installation.

```
docker pull tomsotte/rygel:latest
```

Alternately you can build the image yourself.

```
docker build -t tomsotte/rygel github.com/tomsotte/docker-rygel
```

To build the image on docker-compose, add the `build: .` under `rygel` service.

## Quick Start

Run the image

```
docker run --name=rygel -it --rm --net=host \
    -v /path/to/music:/music \
    -v /path/to/videos:/videos \
    -v /path/to/pictures:/pictures \
    tomsotte/rygel:latest
```

This will start the rygel server and you should now be able to browse the content on DLNA/uPNP compatible devices.

**NOTE:** *You should run the image with host networking so that the server can be discovered over mDNS. (more info: [docs](https://docs.docker.com/engine/userguide/networking/), [article](http://www.dasblinkenlichten.com/docker-networking-101-host-mode/))*

#### Cache store

You should mount a volume at `/root/.cache/rygel` for the cache.

```
docker run --name=rygel -it --rm --net=host \
    -v /path/to/music:/music \
    -v /path/to/videos:/videos \
    -v /path/to/pictures:/pictures \
    -v /path/to/cache:/root/.cache/rygel \
    tomsotte/rygel:latest
```

## Shell Access

For debugging and maintenance purposes you may want access the containers shell. If you are using docker version `1.3.0` or higher you can access a running containers shell using `docker exec` command.

```bash
docker exec -it rygel bash
```

## Upgrading

To upgrade to newer releases, it's simple as this steps:

1. Stop and remove the old container.
2. Pull the latest image and run it as above (same options).

## Docker Compose

Create or use the default `docker-compose.yml` in this repository.

#### docker-compose.yml

```yaml
version: '2'
services:
    rygel:
        image: tomsotte/rygel
        restart: always
        network_mode: host
        volumes:
            - /path/to/music:/music
            - /path/to/videos:/videos
            - /path/to/pictures:/pictures
            - /path/to/cache:/root/.cache/rygel
```
