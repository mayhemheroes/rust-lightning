FROM rustlang/rust:nightly as builder

RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y build-essential clang-11 llvm-11-tools cmake clang curl binutils-dev libbfd-dev libunwind8-dev libblocksruntime-dev pkg-config libudev-dev libssl-dev && \
    ln -s /usr/bin/llvm-config-11 /usr/bin/llvm-config

RUN rustup component add rust-src
RUN cargo install -f honggfuzz

ADD . /rust-lightning
WORKDIR /rust-lightning

RUN cd ./lightning-invoice/fuzz_mayhem && cargo +nightly hfuzz build


# Move Fuzzers to a clean image
FROM ubuntu:20.04

COPY --from=builder rust-lightning/lightning-invoice/fuzz_mayhem/hfuzz_target/x86_64-unknown-linux-gnu/release/serde_data_part /
