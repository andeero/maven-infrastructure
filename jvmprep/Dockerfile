FROM gliderlabs/alpine:3.2

MAINTAINER totto@totto.org

USER root

# Install packages
RUN \
  echo ipv6 >> /etc/modules && \
  apk update && \
  apk upgrade && \
  apk add --update ca-certificates curl

# Install glibc (should be temporary once it's in the base image). https://github.com/gliderlabs/docker-alpine/issues/11#issuecomment-91329401
WORKDIR /tmp
RUN \
  wget "https://circle-artifacts.com/gh/andyshinn/alpine-pkg-glibc/6/artifacts/0/home/ubuntu/alpine-pkg-glibc/packages/x86_64/glibc-2.21-r2.apk" && \
  apk add --allow-untrusted glibc-2.21-r2.apk && \
  wget "https://circle-artifacts.com/gh/andyshinn/alpine-pkg-glibc/6/artifacts/0/home/ubuntu/alpine-pkg-glibc/packages/x86_64/glibc-bin-2.21-r2.apk" && \
  apk add --allow-untrusted glibc-bin-2.21-r2.apk && \
  /usr/glibc/usr/bin/ldconfig /lib /usr/glibc/usr/lib


# Secure the container so that no base users (e.g. root, operator) have a login.
# Provide a script that runs every container invocation (specifically to update Java net.properties).
# Get ready to accept a JVM installation.

ENV JAVA_HOME /usr/local/java
ENV JRE ${JAVA_HOME}/jre
ENV JAVA_OPTS=-Djava.awt.headless=true PATH=${PATH}:${JRE}/bin:${JAVA_HOME}/bin
ENV ENV=/etc/shinit.sh

COPY shinit.sh /etc/

RUN \
  chmod a=rx /etc/shinit.sh && \
  mkdir java

WORKDIR /tmp/java
