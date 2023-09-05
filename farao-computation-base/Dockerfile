FROM eclipse-temurin:17-jre-jammy
LABEL maintainer="Peter Mitri <peter.mitri@rte-france.com>"

ENV TZ=Europe/Paris
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime \
    && echo $TZ > /etc/timezone

RUN apt-get update \
    && apt-get install -y libgomp1\
    && apt-get clean all

RUN groupadd -g 1000 farao \
    && useradd -l -u 1000 -g farao farao \
    && mkdir -p /home/farao/tmp \
    && chown -R farao:farao /home/farao \
    && chmod 777 /home/farao

USER farao

# Download Hades2
RUN curl -sSL https://github.com/rte-france/hades2-distribution/releases/download/V6.7.1/hades2-V6.7.1.1-linux.tar.gz --output /tmp/hades2.tar.gz \
    && mkdir -p /home/farao/farao-dep/runtime/hades2 \
    && tar -xzf /tmp/hades2.tar.gz -C /home/farao/farao-dep/runtime/hades2 --strip-components 1 \
    && rm -f /tmp/hades2.tar.gz

# Download OR-Tools
RUN curl -sSL https://github.com/google/or-tools/releases/download/v9.5/ortools-linux-x86-64-9.5.2237.jar --output /tmp/or-tools.jar \
    && mkdir -p /home/farao/farao-dep/runtime/ortools/lib/ \
    && unzip -j /tmp/or-tools.jar -d /home/farao/farao-dep/runtime/ortools/lib/ ortools-linux-x86-64/* \
    && rm -f /tmp/or-tools.jar

ENV PATH="/home/farao/farao-dep/runtime/hades2/bin:${PATH}" \
    LD_LIBRARY_PATH="/home/farao/farao-dep/runtime/hades2/lib:/home/farao/farao-dep/runtime/ortools/lib:${LD_LIBRARY_PATH}"

