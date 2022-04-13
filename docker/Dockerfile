FROM rust:alpine AS build

RUN apk add --no-cache \
    clang-libs \
    curl \
    libc-dev \
    libnice-dev \
    openssl-dev

ARG REV=docker

WORKDIR /src
RUN curl https://codeload.github.com/rileyinman/mumble-web-proxy/tar.gz/$REV \
    | tar -xzf - --strip-components 1
RUN RUSTFLAGS="-C target-feature=-crt-static" cargo install --path .

FROM alpine

RUN apk add --no-cache \
    bash \
    libgcc \
    libnice

COPY --from=build /usr/local/cargo/bin/mumble-web-proxy /bin/mumble-web-proxy
COPY entrypoint.sh /entrypoint.sh

ENV MWP_LISTEN_WS=64737
ENV MWP_SERVER=mumble-server:64738
ENV MWP_ACCEPT_INVALID_CERTIFICATE=
ENV MWP_ICE_PORT_MIN=
ENV MWP_ICE_PORT_MAX=
ENV MWP_ICE_IPV4=
ENV MWP_ICE_IPV6=

EXPOSE 64737
CMD ["/entrypoint.sh"]
