FROM ubuntu:20.04
LABEL maintainer="Peter Mitri <peter.mitri@rte-france.com>"

ARG FARAO_USER_ID=1000
ARG FARAO_GROUP_ID=1000

RUN groupadd -g ${FARAO_GROUP_ID} farao && useradd -l -u ${FARAO_USER_ID} -g farao farao

ENV TZ=Europe/Paris
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN apt-get update \
    && apt-get install -y libgomp1 unzip openjdk-11-jdk curl\
    && apt-get clean all

RUN mkdir -p /home/farao/farao-dep/runtime/ortools/lib/ \
    && chmod 777 -R /home/farao/farao-dep/runtime/ortools/lib/
RUN mkdir -p /home/farao/farao-dep/runtime/hades2 \
    && chmod 777 -R /home/farao/farao-dep/runtime/hades2

USER farao

# Download Hades2
RUN curl -sSL https://github.com/rte-france/hades2-distribution/releases/download/V6.7.1/hades2-V6.7.1.1-linux.tar.gz --output /tmp/hades2.tar.gz \
    && mkdir -p /home/farao/farao-dep/runtime/hades2 \
    && tar -xzf /tmp/hades2.tar.gz -C /home/farao/farao-dep/runtime/hades2 --strip-components 1 \
    && rm -f /tmp/hades2.tar.gz

# Download OR-Tools
RUN curl -sSL https://github.com/google/or-tools/releases/download/v9.2/ortools-linux-x86-64-9.2.9972.jar --output /tmp/or-tools.jar \
    && mkdir -p /home/farao/farao-dep/runtime/ortools/lib/ \
    && unzip -j /tmp/or-tools.jar -d /home/farao/farao-dep/runtime/ortools/lib/ ortools-linux-x86-64/* \
    && rm -f /tmp/or-tools.jar

ENV PATH="/home/farao/farao-dep/runtime/hades2/bin:${PATH}" \
    LD_LIBRARY_PATH="/home/farao/farao-dep/runtime/hades2/lib:/home/farao/farao-dep/runtime/ortools/lib:${LD_LIBRARY_PATH}"

