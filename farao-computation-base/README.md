![Actions Status](https://github.com/farao-community/docker-images/workflows/FARAO%20computation%20base%20Docker%20Image%20CI/badge.svg)

# FARAO computation base docker image

FARAO computation base Docker image is a base image that embeds Hades2 loadflow and sensitivity computation engine, provided by [RTE France](https://github.com/rte-france/hades2-distribution) as a freeware, and Google OR-Tools optimisation library used by optimisation engines in FARAO. It is designed as a base image for other computation services.

## License warning
First, please note that this image downloads and uses RTE Hades2 distribution. Please read the complete
[license agreement](https://github.com/rte-france/hades2-distribution/blob/master/license.md) before using the executable image.

## Build
For building the docker image on a developer machine, run the following command in a terminal:

```bash
docker build --build-arg FARAO_USER_ID=$(id -u) --build-arg FARAO_GROUP_ID=$(id -g) -t farao/farao-computation-base:latest .
```

## Available build arguments

| Argument name  | Default value | Description                                                                                                        |
| -------------- | :-----------: | ------------------------------------------------------------------------------------------------------------------ |
| FARAO_GROUP_ID | 1000          | GID to give to **farao** group inside the container. Useful for volume sharing between host machine and container. |
| FARAO_USER_ID  | 1000          | UID to give to **farao** user inside the container. Useful for volume sharing between host machine and container.  |
