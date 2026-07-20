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

### [`typst-base`](./docker-compose.yml#L4)

**Description:** A base service defining the core [Typst](https://typst.app/) image. This service isn't intended to be invoked directly, but rather extended and reused by other services (inheritance).

---

### [`typst-fonts`](./docker-compose.yml#L15)

**Description:** Lists the fonts available to the Typst image using the mounted font cache volume.

**Example:**
```bash
docker-compose \
	-f docker-compose.yml \
	run --rm \
	typst-fonts
```

---

### [`typst-info`](./docker-compose.yml#L20)

**Description:** Prints Typst installation and environment information.

**Example:**
```bash
docker-compose \
	-f docker-compose.yml \
	run --rm \
	typst-info
```

---

### [`alpine-volume`](./docker-compose.yml#L25)

**Description:** An interactive Alpine container mounting both the local project directory and the persistent Typst volumes.

The local directory is mounted in `/data`.

The [typst-fonts](#typst-fonts) volume is mounted in `/mnt/typst-fonts`.

The [typst-packages](#typst-packages) volume is mounted in `/mnt/typst-packages`.

**Example:**
```bash
docker-compose \
	--env-file .env \
	-f docker-compose.yml \
	-f docker-compose-util.yml \
	run --rm \
	alpine-volume
```

## Volumes

### [typst-fonts](./docker-compose.yml#L39)

**Description:** Persistent storage for Typst font data.

Access this volume by running the [alpine-volume](#alpine-volume) service.

---

### [typst-packages](./docker-compose.yml#L44)

**Description:** Persistent storage for Typst package cache data.

Access this volume by running the [alpine-volume](#alpine-volume) service.
