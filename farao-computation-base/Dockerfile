FROM eclipse-temurin:17-jre-jammy
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

# Download OR-Tools
RUN curl -sSL https://github.com/google/or-tools/releases/download/v9.5/ortools-linux-x86-64-9.5.2237.jar --output /tmp/or-tools.jar \
    && mkdir -p /home/farao/farao-dep/runtime/ortools/lib/ \
    && unzip -j /tmp/or-tools.jar -d /home/farao/farao-dep/runtime/ortools/lib/ ortools-linux-x86-64/* \
    && rm -f /tmp/or-tools.jar

ENV LD_LIBRARY_PATH="/home/farao/farao-dep/runtime/ortools/lib:${LD_LIBRARY_PATH}"

