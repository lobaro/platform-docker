# Lobaro Platform Installation with Docker

Set up the Lobaro Platform using our ready-to-use container setup.

## Requirements

Additionally to the [System Requirements](https://platform.lobaro.com/docs/installation/requirements), the following requirements apply:

- Docker with [Docker Compose](https://docs.docker.com/compose/).
- [AWS CLI](https://aws.amazon.com/de/cli/) to log in to our Container Registry.

## Installation

To install the platform, proceed as follows:

1. Retrieve a Container Registry login and a Platform license from support@lobaro.com.

1. Open a terminal and run the following command to log in at AWS and the Container Registry:

   ```
   aws configure
   aws ecr get-login-password --region eu-central-1 | docker login --username AWS --password-stdin 072286380570.dkr.ecr.eu-central-1.amazonaws.com
   ```

1. Download the Docker Compose file and `.env` file:

   - With [Traefik](https://traefik.io/traefik/) reverse proxy **(recommended)**: `docker-compose-traefik.yml`
   - Without reverse proxy: `docker-compose-basic.yml`

1. Place your license in `./lobaro-platform/data/license/license.json`

1. Update the `.env` file according to your needs.

1. Start the Docker setup:

   ```
   docker compose up -d
   ```

1. Log in to the platform in your browser. Default credentials:

   - Username: `admin`
   - Password: `admin`

1. Change your e-mail address, name and password on the top right at _admin → Profile Settings_.

Great! You’ve successfully created your Platform with Docker and logged in.

### Next steps

- Change the [Server Configuration](https://platform.lobaro.com/docs/administration/server-configuration) according to your needs.
- Set up [Server Certificates](https://platform.lobaro.com/docs/administration/server-certificates) for CoAPs.
- Configure [Server Monitoring](https://platform.lobaro.com/docs/administration/monitoring) using Prometheus.

## Update

To update the platform, proceed as follows:

1. Bump the `RELEASE=` variable in the `.env` file.

1. Log in to the container registry again. The login provided by AWS ECR is only valid for a short time.

   ```
   aws ecr get-login-password --region eu-central-1 | docker login --username AWS --password-stdin 072286380570.dkr.ecr.eu-central-1.amazonaws.com
   ```

1. Pull container images and restart the affected containers:

   ```
   docker-compose pull && docker-compose up -d
   ```

## Backup

To back up and restore the platform, the following folders need to be saved:

- `lobaro-platform`
- `db-data`

## Uninstall

To uninstall the platform, just stop the Docker setup:

```
docker compose down
```
