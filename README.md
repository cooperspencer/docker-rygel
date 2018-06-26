# Rygel DLNA/uPNP server docker image

<p style="text-align:center">
    <img src="https://wiki.gnome.org/Projects/Rygel?action=AttachFile&do=get&target=Rygel_logo_v1.png" alt="rygel logo" />
</p>

Unofficial docker image for Rygel DLNA/uPNP server docker image. Forked from [sameersbn/docker-rygel](https://github.com/sameersbn/docker-rygel), but since he uses his
own ubuntu docker image, I wanted this to be a more generic image.

**What is Rygel?**

There are many DLNA/UPnP devices on the market, such as the Sony Playstation 3, smartphones, Microsoft XBox 360, DLNA speakers, and TVs from Sony and Samsung, with more expected. ([project homepage](https://wiki.gnome.org/Projects/Rygel/))

## Table of contents

- [Use](#use)
    - [Docker run](#docker-run)
    - [Docker Compose](#docker-compose)
    - [Cache](#cache)
- [Upgrade](#upgrade)
- [Build](#build)

## Use

### Docker run

Quickly run the image via `docker run`.

```
docker run --name=rygel -it --rm --net=host \
    -v /path/to/music:/music \
    -v /path/to/videos:/videos \
    -v /path/to/pictures:/pictures \
    tomsotte/rygel:latest
```

This will start the rygel server and you should now be able to browse the content on DLNA/uPNP compatible devices.

**NOTE:** *You should run the image with host networking so that the server can be discovered over mDNS. (more info: [docs](https://docs.docker.com/engine/userguide/networking/), [article](http://www.dasblinkenlichten.com/docker-networking-101-host-mode/))*

### Docker Compose

Alternatively you can create or use the default `docker-compose.yml` in this repository.

**Example**

```yaml
version: '3'

services:
    rygel:
        image: tomsotte/rygel
        volumes:
            - <path-to-music>:/music
            - <path-to-videos>:/videos
            - <path-to-pictures>:/pictures
            - <path-to-cache:/root/.cache/rygel

```

### Cache

Rygel will make use of the cache. To store it externally you can mount a volume at `/root/.cache/rygel`.

## Upgrade

To upgrade to newer releases, it's simple as this steps:

1. Stop and remove the old container.
2. Pull the latest image and run it as above (same options).

## Build

Automated builds of the image are available on [Dockerhub](https://hub.docker.com/r/tomsotte/rygel) and is the recommended method of installation.

```
docker pull tomsotte/rygel:latest
```

Alternately you can build the image yourself.

```
docker build -t tomsotte/rygel github.com/tomsotte/docker-rygel
```

To build the image on docker-compose, add the `build: <dir-path-to-dockerfile>` under `rygel` service.
