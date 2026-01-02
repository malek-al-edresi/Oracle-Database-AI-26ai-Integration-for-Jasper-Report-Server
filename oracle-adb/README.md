# Oracle ADB Free 26ai on Docker

This directory contains the Docker configuration for running **Oracle Autonomous Database Free 26ai**.

## Prerequisites

- Docker Desktop or Docker Engine
- 4GB+ RAM allocated to Docker (8GB recommended)

## Setup

1.  **Configure Environment Variables**
    Copy the example environment file:
    ```bash
    cp .env.example .env
    ```
    Edit `.env` and set your secure passwords for `WALLET_PASSWORD` and `ADMIN_PASSWORD`.

2.  **Create Docker Assets**
    Run the following commands to create the necessary network and volumes:
    ```bash
    docker network create oracle-adb-network
    docker volume create oracle-adb-data
    docker volume create oracle-adb-logs
    ```

3.  **Start the Container**
    ```bash
    docker compose up -d
    ```

## Access Details

- **Port**: 1521 (Classic), 1522 (TCPS)
- **Hostname**: `oracle-adb` (internal Docker network)
- **Service Name**: `ORCLPDB1` (Default PDB) or `MYATP` (if configured by workload type)

## Connection String Example

```
jdbc:oracle:thin:@localhost:1522/ORCLPDB1
```

## Troubleshooting

- **Startup Time**: The database takes a few minutes to initialize on the first run. Check logs with `docker logs -f oracle-adb-26ai`.
- **Resource Limits**: If the container exits immediately, ensure you have enough memory allocated to Docker.
