FROM golang:1.12.10 AS builder-cockroachoss
RUN apt update > /dev/null \
 && apt install -y g++ libc6-dev libncurses5-dev git bash make cmake autoconf > /dev/null \
 && cd / \
 && wget -qO- https://binaries.cockroachdb.com/cockroach-v19.1.5.src.tgz | tar xz \
 && cd cockroach-v19.1.5 \
 && make buildoss > /dev/null

FROM ubuntu:19.04
COPY --from=builder-cockroachoss /cockroach-v19.1.5/src/github.com/cockroachdb/cockroach/cockroachoss /usr/local/bin/cockroachoss 
RUN apt update > /dev/null \
 && apt install -y vim-tiny libncurses5 > /dev/null \
 && apt-get -y clean > /dev/null \
 && rm -rf /var/lib/apt/lists/* 
WORKDIR /cockroach
ENTRYPOINT ["cockroachoss"]
