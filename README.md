# Local PostgreSQL 18 and pgAdmin Lab

This project provides a ready-to-use solution for quickly deploying an isolated, universal environment for development and experimentation with **PostgreSQL 18** and the **pgAdmin 4** web interface using Docker Compose.

The configuration has been updated to match the new standards of PostgreSQL 18+ images, where data mounting occurs directly into the `/var/lib/postgresql` root, preventing version conflicts and simplifying migrations.

---

## 🚀 Quick Start

### 1. Prerequisites

Ensure that **Docker** and **Docker Compose** are installed and running on your system.

Create a `docker-compose.yml` file in your project directory with the following content:

```yaml
services:
  # PostgreSQL database server
  postgres-db:
    image: postgres:18-alpine
    container_name: universal_postgres
    restart: always
    environment:
      POSTGRES_USER: root
      POSTGRES_PASSWORD: secret_password # Recommended to change to your own secure password
      POSTGRES_DB: default_db
    ports:
      - "5432:5432"
    volumes:
      # Important for PG 18+: mount to /var/lib/postgresql (without /data at the end)
      - pgdata:/var/lib/postgresql

  # Web interface for database management
  pgadmin:
    image: dpage/pgadmin4
    container_name: universal_pgadmin
    restart: always
    environment:
      PGADMIN_DEFAULT_EMAIL: admin@test.com
      PGADMIN_DEFAULT_PASSWORD: admin
    ports:
      - "8080:80"
    depends_on:
      - postgres-db

volumes:
  pgdata:
```
