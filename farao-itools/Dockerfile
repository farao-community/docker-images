FROM maven:3-jdk-8 AS farao-build
ARG BRANCH=master
ADD https://api.github.com/repos/farao-community/farao-core/branches/${BRANCH} /tmp/master.json
RUN git clone -b ${BRANCH} https://github.com/farao-community/farao-core.git \
    && cd farao-core \
    && ./install.sh


FROM centos:8
LABEL maintainer="Sébastien Murgey <sebastien.murgey@rte-france.com>"

ARG FARAO_USER_ID=1774331476
ARG FARAO_GROUP_ID=1774331476

RUN groupadd -g ${FARAO_GROUP_ID} farao && useradd -l -u ${FARAO_USER_ID} -g farao farao

RUN yum update -y \
    && yum install -y libgomp \
    && yum install -y java-1.8.0-openjdk \
    && yum clean all

ENV JAVA_HOME /etc/alternatives/jre

USER farao

# Download Hades2
RUN curl -sSL https://github.com/rte-france/hades2-distribution/releases/download/V6.4.0/hades2-V6.4.0.1.1-linux.tar.gz --output /tmp/hades2.tar.gz \
    && mkdir -p /home/farao/farao-dep/runtime/hades2 \
    && tar -xzf /tmp/hades2.tar.gz -C /home/farao/farao-dep/runtime/hades2 --strip-components 1 \
    && rm -f /tmp/hades2.tar.gz

# Download OR-Tools
RUN curl -sSL https://github.com/google/or-tools/releases/download/v6.10/or-tools_centos-7_v6.10.6025.tar.gz --output /tmp/or-tools.tar.gz \
    && mkdir -p /home/farao/farao-dep/runtime/or-tools \
    && tar -xzf /tmp/or-tools.tar.gz -C /home/farao/farao-dep/runtime/or-tools --strip-components 1 \
    && rm -f /tmp/or-tools.tar.gz \
    && echo "#OR-Tools config" >> ~/.bashrc \
    && echo "export OR_TOOLS_HOME=/home/farao/farao-dep/runtime/or-tools" >> ~/.bashrc \
    && echo "export PATH=$PATH:$OR_TOOLS_HOME/bin" >> ~/.bashrc \
    && echo "export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$OR_TOOLS_HOME/lib" >> ~/.bashrc

COPY --from=farao-build --chown=farao:farao /root/farao /home/farao/farao


ENTRYPOINT ["/home/farao/farao/bin/itools"]
