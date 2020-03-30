[![Actions Status](https://github.com/farao-community/docker-images/workflows/CI/badge.svg)](https://github.com/farao-community/docker-images/workflows/FARAO%20iTools%20Docker%20Image%20CI/badge.svg)

# FARAO itools docker image

FARAO itools Docker image is an executable image that embeds FARAO tools developed for [PowSyBl iTools](https://www.powsybl.org/docs/tools/).

It is a [two-stage](https://docs.docker.com/develop/develop-images/multistage-build/) built image that compiles and deploy FARAO tools in
an iTools package available as the image entrypoint.

## License warning
First, please note that this image downloads and uses RTE Hades2 distribution. Please read the complete
[license agreement](https://github.com/rte-france/hades2-distribution/blob/master/license.md) before using the executable image.

## Build
For building the docker image on a developer machine, run the following command in a terminal:

```bash
docker build --build-arg FARAO_USER_ID=$(id -u) --build-arg FARAO_GROUP_ID=$(id -g) -t farao/itools:latest .
```

## Run
After building the docker image, it is possible to use it to run RAO computation.
For runing a RAO computation on *network.uct* network file and *crac.json* CRAC file, run the following command in a terminal, considering the files are saved in current directory:

```bash
docker run --rm -v $(pwd):/work farao/itools:latest rao --case-file /work/network.uct --crac-file /work/crac.json --output-format JSON --output-file /work/output.json
```

The result will be saved in *output.json* file in current directory.

## Available build arguments

| Argument name  | Default value | Description                                                                                                        |
| -------------- | :-----------: | ------------------------------------------------------------------------------------------------------------------ |
| BRANCH         | master        | Development branch or tag of farao-core to be compiled and deployed in the container.                              |
| FARAO_GROUP_ID | 1000          | GID to give to **farao** group inside the container. Useful for volume sharing between host machine and container. |
| FARAO_USER_ID  | 1000          | UID to give to **farao** user inside the container. Useful for volume sharing between host machine and container.  |

