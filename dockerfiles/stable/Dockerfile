FROM debian:stretch-slim

LABEL maintainer="Liu Chong <mail@clojure.cn>"
LABEL collaborator="Christopher Brown <cb@skeptomai.com>"

WORKDIR /root

# common packages
RUN apt-get update && \
    apt-get install --no-install-recommends -y \
    ca-certificates curl file \
    build-essential \
    autoconf automake autotools-dev libtool xutils-dev \
    lsb-release nano sudo cmake git\
    #    linux-cloud-tools-generic-hwe-18.04-edge linux-tools-generic linux-tools-generic-hwe-18.04-edge && \
    linux-perf linux-base && \
    rm -rf /var/lib/apt/lists/*

ENV SSL_VERSION=1.0.2s

RUN curl https://www.openssl.org/source/openssl-$SSL_VERSION.tar.gz -O && \
    tar -xzf openssl-$SSL_VERSION.tar.gz && \
    cd openssl-$SSL_VERSION && ./config && make depend && make install && \
    cd .. && rm -rf openssl-$SSL_VERSION*

ENV OPENSSL_LIB_DIR=/usr/local/ssl/lib \
    OPENSSL_INCLUDE_DIR=/usr/local/ssl/include \
    OPENSSL_STATIC=1

# RUN ln -sfn `which perf_4.9` /usr/bin/perf
RUN useradd --uid 5000 --create-home --shell /bin/bash dev

RUN adduser dev sudo
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

ENV USER=dev
WORKDIR /home/dev
USER dev

# install toolchain
RUN curl https://sh.rustup.rs -sSf | \
    sh -s -- --default-toolchain stable -y

ENV PATH=/home/dev/.cargo/bin:$PATH

RUN cargo install flamegraph

# mkdir Projects && cd Projects && cargo new testapp
# docker run --security-opt seccomp=seccomp.json --rm --privileged -it --name dispensable_air -v $(pwd)/Projects:/Projects dockerrustup-stable:latest
# cd /Projects/testapp && cargo build && cargo flamegraph
# /proc/sys/kernel/kptr_restrict
# /proc/sys/kernel/perf_event_paranoid
# https://stackoverflow.com/questions/20390601/is-there-a-way-to-set-kptr-restrict-to-0
# https://stackoverflow.com/questions/38927895/how-do-you-get-debugging-symbols-working-in-linux-perf-tool-inside-docker-contai
# https://www.bretfisher.com/docker-for-mac-commands-for-getting-into-local-docker-vm/ 
# https://www.reddit.com/r/rust/comments/a1drss/how_to_debug_cargo_build_preject/
# http://www.brendangregg.com/perf.html
# https://gendignoux.com/blog/2019/11/09/profiling-rust-docker-perf.html
# https://blog.anp.lol/rust/2016/07/24/profiling-rust-perf-flamegraph/
# https://github.com/alexcrichton/cargo-edit-locally