FROM rust:alpine AS builder
RUN apk add --no-cache musl-dev
WORKDIR /app

COPY core/sid/Cargo.toml core/sid/Cargo.lock* ./
COPY core/sid/src ./src

RUN --mount=type=cache,target=/usr/local/cargo/registry \
    --mount=type=cache,target=/app/target \
    cargo build --release && \
    cp /app/target/release/vtfk-sid /vtfk-sid-bin

FROM scratch
COPY --from=builder /vtfk-sid-bin /vtfk-sid
ENTRYPOINT ["/vtfk-sid"]
