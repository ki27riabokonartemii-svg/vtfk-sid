# Етап 1: Збірка (Builder)
FROM rust:alpine AS builder
RUN apk add --no-cache musl-dev
WORKDIR /app

# Ранній шар: копіюємо лише конфігурацію залежностей
COPY core/sid/Cargo.toml core/sid/Cargo.lock* ./

# Прогрів кешу: компіляція порожнього коду для створення бібліотечного кешу
RUN mkdir src && \
    echo "fn main() {}" > src/main.rs && \
    cargo build --release && \
    rm -rf src

# Копіюємо реальний вихідний код проєкту
COPY core/sid/src ./src

# Компіляція власного бінарника
RUN touch src/main.rs && cargo build --release

# Етап 2: Scratch
FROM scratch
COPY --from=builder /app/target/release/vtfk-sid /vtfk-sid
ENTRYPOINT ["/vtfk-sid"]
