# My Environment Setup with Docker Compose

This project provides a full-stack development environment using Docker Compose. It includes services for PHP, Nginx, MySQL, PostgreSQL, MongoDB, Redis, and management tools like Adminer and Mongo Express.

## Table of Contents

- [My Environment Setup with Docker Compose](#my-environment-setup-with-docker-compose)
  - [Table of Contents](#table-of-contents)
  - [Project Structure](#project-structure)
  - [Services](#services)
    - [Service Descriptions](#service-descriptions)
  - [Volumes](#volumes)
  - [Networks](#networks)
  - [Environment Variables](#environment-variables)
  - [Usage](#usage)
    - [1. Build and Start the Containers](#1-build-and-start-the-containers)
    - [2. Access the Services](#2-access-the-services)
    - [3. Stopping the Containers](#3-stopping-the-containers)
  - [Accessing Services](#accessing-services)
  - [Custom Configuration](#custom-configuration)
  - [Troubleshooting](#troubleshooting)
  - [Contributing](#contributing)
  - [License](#license)

## Project Structure

```structure
.
├── docker-compose.yml
├── Dockerfile-php
├── nginx.conf
└── src/
    └── (your PHP application files)
```

- **docker-compose.yml**: Defines the services, volumes, and networks.
- **Dockerfile-php**: PHP Dockerfile to install necessary extensions.
- **nginx.conf**: Nginx configuration file.
- **src/**: Directory containing your PHP application source code.

## Services

- **PHP**: PHP 8.3 FPM with MongoDB extension.
- **Nginx**: Web server to serve your PHP application.
- **MySQL**: Database service for MySQL.
- **PostgreSQL**: Database service for PostgreSQL.
- **MongoDB**: NoSQL database service.
- **Redis**: In-memory data structure store.
- **Adminer**: Database management tool.
- **Mongo Express**: Web-based MongoDB admin interface.

### Service Descriptions

1. **PHP**

   - **Build Context**: `Dockerfile-php`
   - **Container Name**: `my-php-container`
   - **Volumes**: Maps `./src` to `/var/www/html`
   - **Network**: `app-network`
   - **Depends on**: MySQL, PostgreSQL, MongoDB
   - **Entrypoint**: `php-fpm`

2. **Nginx**

   - **Image**: `nginx:latest`
   - **Container Name**: `my-nginx-container`
   - **Volumes**: Maps `./src` to `/var/www/html`, `./nginx.conf` to `/etc/nginx/nginx.conf`
   - **Ports**: `80:80`
   - **Network**: `app-network`
   - **Depends on**: PHP

3. **MySQL**

   - **Image**: `mysql:latest`
   - **Container Name**: `my-mysql-container`
   - **Environment Variables**: Configured via `.env`
   - **Ports**: `3306`
   - **Network**: `app-network`

4. **PostgreSQL**

   - **Image**: `postgres:latest`
   - **Container Name**: `my-postgres-container`
   - **Environment Variables**: Configured via `.env`
   - **Ports**: `5432`
   - **Network**: `app-network`

5. **MongoDB**

   - **Image**: `mongo:latest`
   - **Container Name**: `my-mongo-container`
   - **Volumes**: Persistent storage for MongoDB data and logs.
   - **Environment Variables**: Configured via `.env`
   - **Ports**: `27017`
   - **Network**: `app-network`

6. **Redis**

   - **Image**: `redis:latest`
   - **Container Name**: `my-redis-container`
   - **Ports**: `6379`
   - **Network**: `app-network`

7. **Adminer**

   - **Image**: `adminer:latest`
   - **Container Name**: `my-adminer-container`
   - **Ports**: `8080`
   - **Network**: `app-network`
   - **Depends on**: MySQL, PostgreSQL, MongoDB

8. **Mongo Express**
   - **Image**: `mongo-express:latest`
   - **Container Name**: `mongo-express`
   - **Ports**: `8081`
   - **Environment Variables**: Configured via `.env`
   - **Network**: `app-network`
   - **Depends on**: MongoDB

## Volumes

- **mongodb-data**: Persists MongoDB data.
- **mongodb-log**: Stores MongoDB logs.

## Networks

- **app-network**: A bridge network to connect all services.

## Environment Variables

Environment variables are defined in the `.env` file:

```plaintext
# MySQL Configuration
MYSQL_USER=root
MYSQL_ROOT_PASSWORD=mysqlpass
MYSQL_DATABASE=myqsl_db

# PostgreSQL Configuration
POSTGRES_USER=root
POSTGRES_PASSWORD=postgrespass
POSTGRES_DB=postgre_db

# MongoDB Configuration
MONGO_INITDB_ROOT_USERNAME=root
MONGO_INITDB_ROOT_PASSWORD=mongopass
MONGO_SERVER=mongodb

# MongoDB Express Configuration
MONGO_EXPRESS_USERNAME=root
MONGO_EXPRESS_PASSWORD=mongopass

# Ports
MYSQL_PORT=3306
POSTGRES_PORT=5432
MONGO_PORT=27017
REDIS_PORT=6379
ADMINER_PORT=8080
MONGO_EXPRESS_PORT=8081
```

## Usage

### 1. Build and Start the Containers

```bash
docker-compose up --build -d
```

This command will build the necessary images and start the containers in detached mode.

### 2. Access the Services

- **PHP Application**: `http://localhost/`
- **Adminer**: `http://localhost:8080/`
- **Mongo Express**: `http://localhost:8081/`

### 3. Stopping the Containers

```bash
docker-compose down
```

This command stops and removes all containers defined in `docker-compose.yml`.

## Accessing Services

- **Adminer**: A web-based database management tool. Use it to manage MySQL, PostgreSQL, and MongoDB databases.
- **Mongo Express**: A web-based interface for MongoDB. It provides an easy way to interact with MongoDB databases.

## Custom Configuration

- **Nginx**: Modify the `nginx.conf` file for custom server configurations.
- **PHP**: Extend the `Dockerfile-php` to install additional PHP extensions or modify the PHP setup.

## Troubleshooting

- **Ports Already in Use**: If you encounter port conflicts, modify the port mappings in the `.env` file.
- **Containers Not Starting**: Run `docker-compose logs` to view the logs for troubleshooting issues.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request or open an issue.

## License

This project is licensed under the MIT License.
