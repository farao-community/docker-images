![Actions Status](https://github.com/farao-community/docker-images/workflows/FARAO%20Hades2%20Docker%20Image%20CI/badge.svg)

# FARAO Hades2 docker image

FARAO Hades2 Docker image is an executable image that embeds Hades2 loadflow and sensitivity computation engine, provided by [RTE France](https://github.com/rte-france/hades2-distribution) as a freeware. It is designed as a base image for other computation services, using Hades2 as loadflow engine.

## License warning
First, please note that this image downloads and uses RTE Hades2 distribution. Please read the complete
[license agreement](https://github.com/rte-france/hades2-distribution/blob/master/license.md) before using the executable image.

## Build
For building the docker image on a developer machine, run the following command in a terminal:

```bash
docker build --build-arg FARAO_USER_ID=$(id -u) --build-arg FARAO_GROUP_ID=$(id -g) -t farao/hades2-base:latest .
```

## Available build arguments

| Argument name  | Default value | Description                                                                                                        |
| -------------- | :-----------: | ------------------------------------------------------------------------------------------------------------------ |
| FARAO_GROUP_ID | 1000          | GID to give to **farao** group inside the container. Useful for volume sharing between host machine and container. |
| FARAO_USER_ID  | 1000          | UID to give to **farao** user inside the container. Useful for volume sharing between host machine and container.  |
