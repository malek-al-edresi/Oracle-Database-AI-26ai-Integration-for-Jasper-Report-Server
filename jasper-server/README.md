# JasperReports Server Community Edition

This directory contains the Docker configuration for **JasperReports Server (Community Edition)** backed by **MariaDB**.

## Prerequisites

- Docker Desktop or Docker Engine
- Oracle ADB Stack (running if you plan to connect to it)

## Setup

1.  **Configure Environment Variables**
    Copy the example environment file:
    ```bash
    cp .env.example .env
    ```
    *Note: The default credentials are `jasperadmin` / `jasperadmin`. You can change them in `.env`.*

2.  **Start the Stack**
    ```bash
    docker compose up -d
    ```

3.  **Access the Interface**
    - **URL**: `http://localhost:9091/jasperserver`
    - **Username**: `jasperadmin` (or as configured)
    - **Password**: `jasperadmin` (or as configured)

## Connecting to Oracle ADB 26ai

To use the Oracle Autonomous Database as a data source, you need to add the Oracle JDBC driver (`ojdbc8.jar`) to the JasperReports container.

### Step 1: Download Driver
Download `ojdbc8.jar` (and `osdt_cert.jar`, `oraclepki.jar` if using Wallet with mutual TLS, though 26ai usually supports TLS without wallet for some connections, wallet is safer).
*For this setup, standard `ojdbc8.jar` is usually sufficient.*

### Step 2: Copy Driver to Container
After the container is running:

```bash
docker cp /path/to/your/ojdbc8.jar jasperreports:/opt/bitnami/jasperreports/apache-tomcat/lib/
docker restart jasperreports
```

### Step 3: Create Data Source
In the JasperReports Web UI:
1.  Right-click "Data Sources" -> "Add Resource" -> "Data Source".
2.  **Type**: JDBC Data Source.
3.  **Driver**: Select "Oracle (oracle.jdbc.OracleDriver)".
4.  **URL**:
    ```text
    jdbc:oracle:thin:@oracle-adb:1522/ORCLPDB1
    ```
    *Note: `oracle-adb` is the hostname defined in the Oracle stack's network.*
5.  **Username/Password**: Your Oracle ADB credentials (e.g., `ADMIN`).

## Troubleshooting

- **Login Failed**: Ensure the database container (`mariadb`) is fully up before `jasperreports` tries to connect. Docker Compose handles startup order but not "ready" state. If it fails, wait 30s and run `docker restart jasperreports`.
