FROM golang:1.12.5 AS builder-cockroachoss
RUN apt update > /dev/null \
 && apt install -y g++ libncurses5-dev git bash make cmake autoconf > /dev/null \
 && cd / \
 && wget -qO- https://binaries.cockroachdb.com/cockroach-v19.1.0.src.tgz | tar xz \
 && cd cockroach-v19.1.0 \
 && make buildoss > /dev/null

FROM ubuntu:19.04
COPY --from=builder-cockroachoss /cockroach-v19.1.0/src/github.com/cockroachdb/cockroach/cockroachoss /usr/local/bin/cockroachoss 
RUN apt update > /dev/null \
 && apt install -y vim-tiny libncurses5 > /dev/null \
 && apt-get -y clean > /dev/null \
 && rm -rf /var/lib/apt/lists/* 
ENTRYPOINT ["cockroachoss"]