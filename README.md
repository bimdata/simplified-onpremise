# simplified-onpremise

This project use Docker & Docker Compose to setup a BIMData API.

## Prerequisites
  - Docker
  - Docker compose
  - Access to our Docker registry to be able to download the images

## Usage

On all the servers:
  - Clone or copy this repository on each server.
  - Copy the `.env.example` file to `.env`.
  - Modify the `.env` to match your needs. The file should be the same on all servers.

On the application server (the one running the API), run the following commands:
  - `mkdir -p data/api_storage`
  - `docker compose up -d`

On the the worker servers:
  - `docker compose -f compose.worker.yaml up -d`

### TLS
If you need to setup TLS, on the application server:
  - `mkdir -p config/certs`.
  - Create the file `config/certs/fullchain.pem` containing the certificate and all the needed intermediate certificates.
  - Create the file `config/certs/privkey.pem` containing the private key.
  - Modify `compose.yml`, in the `nginx-proxy` configuration, you need to uncomment some lines as instructed by some comments.
  - Modify `config\nginx.conf` to uncomment the needed include (the is a comment that indicate which line)
  - `docker compose up -d`
