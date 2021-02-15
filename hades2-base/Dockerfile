FROM fedora:31
LABEL maintainer="Sébastien Murgey <sebastien.murgey@rte-france.com>"

ARG FARAO_USER_ID=1000
ARG FARAO_GROUP_ID=1000

RUN groupadd -g ${FARAO_GROUP_ID} farao && useradd -l -u ${FARAO_USER_ID} -g farao farao

RUN yum update -y \
    && yum install -y libgomp \
    && yum clean all

USER farao

# Download Hades2
RUN curl -sSL https://github.com/rte-france/hades2-distribution/releases/download/V6.7.1/hades2-V6.7.1.1-linux.tar.gz --output /tmp/hades2.tar.gz \
    && mkdir -p /home/farao/farao-dep/runtime/hades2 \
    && tar -xzf /tmp/hades2.tar.gz -C /home/farao/farao-dep/runtime/hades2 --strip-components 1 \
    && rm -f /tmp/hades2.tar.gz

ENV PATH="/home/farao/farao-dep/runtime/hades2/bin:${PATH}" \
    LD_LIBRARY_PATH="/home/farao/farao-dep/runtime/hades2/lib:${LD_LIBRARY_PATH}"

ENTRYPOINT ["/home/farao/farao-dep/runtime/hades2/hades2.sh"]
