FROM eclipse-temurin:21-jre-jammy
LABEL maintainer="Peter Mitri <peter.mitri@rte-france.com>"

RUN apt-get update \
    && apt-get install -y libgomp1\
    && apt-get install -y unzip\
    && apt-get clean all

RUN groupadd -g 1000 farao \
    && useradd -l -u 1000 -g farao farao \
    && mkdir -p /home/farao/tmp \
    && chown -R farao:farao /home/farao \
    && chmod 777 /home/farao

USER farao
