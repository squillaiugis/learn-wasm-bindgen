FROM rust:latest

RUN apt-get update && apt-get install -y \
    git \
    curl \
    wget \
    build-essential \
    lldb \
    binaryen \
    && rm -rf /var/lib/apt/lists/*

RUN rustup component add rustfmt clippy rust-src
RUN cargo install wasm-bindgen-cli
RUN cargo install wasm-pack

RUN --mount=type=ssh,uid=1000

WORKDIR /workspace

SHELL ["/bin/bash", "-c"] 