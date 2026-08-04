# compose-typst

## Introduction

This project provides several Docker Compose services for Typst workflows.

The services support the following Typst-related tasks:

- inspecting available fonts
- checking Typst installation information
- managing persistent cache data

Docker Compose permits this without installing Typst directly on your local machine.

## Variables

The utility services require the following environment variables:

- `UID` - The Unix User ID. This can be obtained by running `id -u`
- `GID` - The Unix User Group ID. This can be obtained by running `id -g`

In the following examples, the `UID` and `GID` values are stored in an `.env` file passed to Docker Compose using the `--env-file` argument.

## Services

### [`typst-base`](./compose-typst/docker-compose.yml#L4)

**Description:** A base service defining the core [Typst](https://typst.app/) image. This service isn't intended to be invoked directly, but rather extended and reused by other services (inheritance).

---

### [`typst-fonts`](./compose-typst/docker-compose.yml#L15)

**Description:** Lists the fonts available to the Typst image using the mounted font cache volume.

The `typst-fonts` service can be started with [fonts.sh](./fonts.sh).

**Example:**
```bash
docker compose \
    --env-file .env \
    -f compose-typst/docker-compose.yml \
    run --rm \
    typst-fonts
```

---

### [`typst-info`](./compose-typst/docker-compose.yml#L20)

**Description:** Prints Typst installation and environment information.

The `typst-info` service can be started with [info.sh](./info.sh).

**Example:**
```bash
docker compose \
    --env-file .env \
    -f compose-typst/docker-compose.yml \
    run --rm \
    typst-info
```

---

### [`alpine-volume`](./compose-typst/docker-compose.yml#L25)

**Description:** An interactive Alpine container mounting both the local project directory and the persistent Typst volumes.

- The local directory is mounted in `/data`.
- The [typst-packages](#typst-packages) volume is mounted in `/mnt/typst-packages`.

The `alpine-volume` service can be started with [volume.sh](./volume.sh).

**Example:**
```bash
docker compose \
    --env-file .env \
    -f compose-typst/docker-compose.yml \
    run --rm \
    alpine-volume
```

## Volumes

### [typst-packages](./compose-typst/docker-compose.yml#L37)

**Description:** Persistent storage for Typst package cache data.

Access this volume by running the [alpine-volume](#alpine-volume) service.
