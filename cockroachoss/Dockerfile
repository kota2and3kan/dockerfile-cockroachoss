FROM golang:1.15.11 AS builder-cockroachoss
RUN apt update > /dev/null \
 && apt install -y g++ libc6-dev libncurses5-dev git bash make cmake autoconf > /dev/null \
 && cd / \
 && wget -qO- https://binaries.cockroachdb.com/cockroach-v21.2.0.src.tgz | tar xz \
 && cd cockroach-v21.2.0 \
 && make buildoss > /dev/null

FROM ubuntu:20.04
COPY --from=builder-cockroachoss /cockroach-v21.2.0/src/github.com/cockroachdb/cockroach/cockroachoss /usr/local/bin/cockroachoss 
RUN apt update > /dev/null \
 && DEBIAN_FRONTEND=noninteractive apt install -y vim-tiny libncurses5 tzdata > /dev/null \
 && apt-get -y clean > /dev/null \
 && rm -rf /var/lib/apt/lists/*
WORKDIR /cockroach
ENTRYPOINT ["cockroachoss"]
