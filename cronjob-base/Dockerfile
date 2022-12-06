FROM alpine:3.17.0
LABEL maintainer="Vincent BOCHET <vincent.bochet@rte-france.com>"

ENV TZ=Europe/Paris

RUN apk update
RUN apk add tzdata
RUN apk add curl

RUN crond
