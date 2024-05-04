# PostgreSQL with Multiple Extensions Docker Image

> Reference: [cloudnative-pg/postgres-containers](https://github.com/cloudnative-pg/postgres-containers)

This Docker image is based on the official [CloudNative](https://github.com/cloudnative-pg/cloudnative-pg) PostgreSQL image (ghcr.io/cloudnative-pg/postgresql) using Debian as the base OS.

- The official image include the following package:
  - Barman Cloud
  - PGAudit
  - Postgres Failover Slots
  - pgvector
- The image of this repository adds the following packages:
  - TimescaleDB
  - PGroonga
  - system_stats

## Getting Started

To use this Docker image, you must have Docker installed on your machine. If you do not have Docker installed, follow the instructions on the [official Docker documentation](https://docs.docker.com/get-docker/) to get started.

### Pulling the Image

This image isn't hosted on a public registry but can be built directly using the provided Dockerfile.

### Building the Image

To build the image:

1. Save the Dockerfile to a directory on your computer.
2. Open a terminal and navigate to the directory containing the Dockerfile.
3. Run the following command to build the image:
   ```bash
   docker build -t pg
   ```

