FROM maven:3-jdk-11 AS farao-build
ARG BRANCH=master
ADD https://api.github.com/repos/farao-community/farao-core/branches/${BRANCH} /tmp/master.json
RUN git clone -b ${BRANCH} https://github.com/farao-community/farao-core.git \
    && cd farao-core \
    && ./install.sh


FROM fedora:31
LABEL maintainer="Sébastien Murgey <sebastien.murgey@rte-france.com>"

ARG FARAO_USER_ID=1000
ARG FARAO_GROUP_ID=1000

RUN groupadd -g ${FARAO_GROUP_ID} farao && useradd -l -u ${FARAO_USER_ID} -g farao farao

RUN yum update -y \
    && yum install -y libgomp unzip java-11-openjdk \
    && yum clean all

ENV JAVA_HOME /etc/alternatives/jre

USER farao

# Download Hades2
RUN curl -sSL https://github.com/rte-france/hades2-distribution/releases/download/V6.7.1/hades2-V6.7.1.1-linux.tar.gz --output /tmp/hades2.tar.gz \
    && mkdir -p /home/farao/farao-dep/runtime/hades2 \
    && tar -xzf /tmp/hades2.tar.gz -C /home/farao/farao-dep/runtime/hades2 --strip-components 1 \
    && rm -f /tmp/hades2.tar.gz

# Download OR-Tools
RUN curl -sSL https://search.maven.org/remotecontent?filepath=com/farao-community/ortools/ortools-linux-x86-64/7.8.7959/ortools-linux-x86-64-7.8.7959.jar --output /tmp/or-tools.jar \
    && mkdir -p /home/farao/farao-dep/runtime/ortools/lib/ \
    && unzip -j /tmp/or-tools.jar -d /home/farao/farao-dep/runtime/ortools/lib/ linux-x86-64/* \
    && rm -f /tmp/or-tools.jar

COPY --from=farao-build --chown=farao:farao /root/farao /home/farao/farao

ENV PATH="/home/farao/farao-dep/runtime/hades2/bin:${PATH}" \
    LD_LIBRARY_PATH="/home/farao/farao-dep/runtime/hades2/lib:/home/farao/farao-dep/runtime/ortools/lib:${LD_LIBRARY_PATH}"

ENTRYPOINT ["/home/farao/farao/bin/itools"]
