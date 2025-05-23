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
  - Modify `config\nginx.conf` to uncomment the needed include (there is a comment that indicate which line)
  - `docker compose up -d`

## Configuration
This is all the variable that can be set in the `.env` file.

| Variables                      | Default value              | Description                                                                         |
|--------------------------------|----------------------------|-------------------------------------------------------------------------------------|
| VERSION                        |                            | Version of BIMData that will be installed.                                          |
||||
| API_DNS_NAME                   |                            | DNS name use to contact the API.                                                    |
| API_URL                        | https://${API_DNS_NAME}    | API URL (with the scheme, must be change if no TLS enabled)                         |
||||
| API_MASTER_TOKEN               |                            | Secret use by the API. Must be define.                                              |
| API_SECRET_KEY                 |                            | Secret use by the API. Must be define.                                              |
||||
| API_DB_HOST                    |                            | Host use by the API to connect to the database.                                     |
| API_DB_PORT                    |                            | Port use by the API to connect to the database.                                     |
| API_DB_NAME                    |                            | Database name use by the API to connect to the database.                            |
| API_DB_USER                    |                            | User use by the API to connect to the database.                                     |
| API_DB_PASSWORD                |                            | Password use by the API to connect to the database.                                 |
||||
| RABBITMQ_HOST                  |                            | Host use by the API & the workers to connect to the rabbitmq.                       |
| RABBITMQ_PORT                  |                            | Port use by the API & the workers to connect to the rabbitmq.                       |
| RABBITMQ_VHOST                 | /                          | NOT IMPLEMENTED YET                                                                 |
| RABBITMQ_USER                  |                            | User use by the API & the workers to connect to the rabbitmq.                       |
| RABBITMQ_PASSWORD              |                            | Password use by the API & the workers to connect to the rabbitmq.                   |
||||
| SMTP_HOST                      |                            | SMTP server address.                                                                |
| SMTP_PORT                      | 587                        | SMTP server port.                                                                   |
| SMTP_USER                      |                            | User use for the authentication on the SMTP server.                                 |
| SMTP_PASS                      |                            | Password use for the authentication on the SMTP server.                             |
| SMTP_USE_TLS                   | true                       | If the SMTP connection should use STARTTLS or not.                                  |
| DEFAULT_FROM_EMAIL             |                            | Email address use as default sender.                                                |
| DEBUG_MAIL_TO                  |                            | Email recipient use to send application error / stacktrace.                         |
||||
| DOCKER_REGISTRY                | docker-registry.bimdata.io | Address of the docker registry from where BIMData application app pulled.           |
| DOCKER_REGISTRY_IMAGE_PATH     | on-premises/               | Path in the docker registry address betfore the image name.                         |
||||
| WORKERS_B2D_TIMEOUT            | 1800                       | Timeout for a b2d process.                                                          |
| WORKERS_B2D_REPLICA            | 1                          | Number of replicas deployed on *each* worker server.                                |
| WORKERS_B2D_CPU                | 1                          | Number of CPUs allocated for each replicas.                                         |
| WORKERS_B2D_MEMORY             | 6g                         | Quantity of RAM allocated for each replicas.                                        |
||||
| WORKERS_DWG_GEOMETRIES_TIMEOUT | 14400                      | Timeout for a dwg geometry process.                                                 |
| WORKERS_DWG_GEOMETRIES_REPLICA | 1                          | Number of replicas deployed on *each* worker server.                                |
| WORKERS_DWG_GEOMETRIES_CPU     | 1                          | Number of CPUs allocated for each replicas.                                         |
| WORKERS_DWG_GEOMETRIES_MEMORY  | 16g                        | Quantity of RAM allocated for each replicas.                                        |
||||
| WORKERS_DWG_PROPERTIES_TIMEOUT | 3600                       | Timeout for a dwg property process.                                                 |
| WORKERS_DWG_PROPERTIES_REPLICA | 1                          | Number of replicas deployed on *each* worker server.                                |
| WORKERS_DWG_PROPERTIES_CPU     | 1                          | Number of CPUs allocated for each replicas.                                         |
| WORKERS_DWG_PROPERTIES_MEMORY  | 4g                         | Quantity of RAM allocated for each replicas.                                        |
||||
| WORKERS_ELEVATION_TIMEOUT      | 900                        | Timeout for an elevation process.                                                   |
| WORKERS_ELEVATION_REPLICA      | 1                          | Number of replicas deployed on *each* worker server.                                |
| WORKERS_ELEVATION_CPU          | 1                          | Number of CPUs allocated for each replicas.                                         |
| WORKERS_ELEVATION_MEMORY       | 4g                         | Quantity of RAM allocated for each replicas.                                        |
||||
| WORKERS_EXPORT_TIMEOUT         | 7200                       | Timeout for an export process.                                                      |
| WORKERS_EXPORT_REPLICA         | 1                          | Number of replicas deployed on *each* worker server.                                |
| WORKERS_EXPORT_CPU             | 1                          | Number of CPUs allocated for each replicas.                                         |
| WORKERS_EXPORT_MEMORY          | 4g                         | Quantity of RAM allocated for each replicas.                                        |
||||
| WORKERS_EXTRACT_TIMEOUT        | 7200                       | Timeout for an extract process.                                                     |
| WORKERS_EXTRACT_REPLICA        | 1                          | Number of replicas deployed on *each* worker server.                                |
| WORKERS_EXTRACT_CPU            | 1                          | Number of CPUs allocated for each replicas.                                         |
| WORKERS_EXTRACT_MEMORY         | 4g                         | Quantity of RAM allocated for each replicas.                                        |
||||
| WORKERS_GLTF_TIMEOUT           | 36000                      | Timeout for a gltf process.                                                         |
| WORKERS_GLTF_REPLICA           | 1                          | Number of replicas deployed on *each* worker server.                                |
| WORKERS_GLTF_CPU               | 1                          | Number of CPUs allocated for each replicas.                                         |
| WORKERS_GLTF_MEMORY            | 32g                        | Quantity of RAM allocated for each replicas.                                        |
||||
| WORKERS_MERGE_TIMEOUT          | 7200                       | Timeout for a merge process.                                                        |
| WORKERS_MERGE_REPLICA          | 1                          | Number of replicas deployed on *each* worker server.                                |
| WORKERS_MERGE_CPU              | 1                          | Number of CPUs allocated for each replicas.                                         |
| WORKERS_MERGE_MEMORY           | 4g                         | Quantity of RAM allocated for each replicas.                                        |
||||
| WORKERS_POINTCLOUD_TIMEOUT     | 36000                      | Timeout for a pointcloud process.                                                   |
| WORKERS_POINTCLOUD_REPLICA     | 1                          | Number of replicas deployed on *each* worker server.                                |
| WORKERS_POINTCLOUD_CPU         | 1                          | Number of CPUs allocated for each replicas.                                         |
| WORKERS_POINTCLOUD_MEMORY      | 32g                        | Quantity of RAM allocated for each replicas.                                        |
||||
| WORKERS_PREVIEW_2D_TIMEOUT     | 600                        | Timeout for a 2D preview process.                                                   |
| WORKERS_PREVIEW_2D_REPLICA     | 1                          | Number of replicas deployed on *each* worker server.                                |
| WORKERS_PREVIEW_2D_CPU         | 1                          | Number of CPUs allocated for each replicas.                                         |
| WORKERS_PREVIEW_2D_MEMORY      | 4g                         | Quantity of RAM allocated for each replicas.                                        |
||||
| WORKERS_PREVIEW_3D_TIMEOUT     | 600                        | Timeout for a 3D preview process.                                                   |
| WORKERS_PREVIEW_3D_REPLICA     | 1                          | Number of replicas deployed on *each* worker server.                                |
| WORKERS_PREVIEW_3D_CPU         | 1                          | Number of CPUs allocated for each replicas.                                         |
| WORKERS_PREVIEW_3D_MEMORY      | 8g                         | Quantity of RAM allocated for each replicas.                                        |
||||
| WORKERS_PREVIEW_OFFICE_TIMEOUT | 600                        | Timeout for an office preview process.                                              |
| WORKERS_PREVIEW_OFFICE_REPLICA | 1                          | Number of replicas deployed on *each* worker server.                                |
| WORKERS_PREVIEW_OFFICE_CPU     | 1                          | Number of CPUs allocated for each replicas.                                         |
| WORKERS_PREVIEW_OFFICE_MEMORY  | 4g                         | Quantity of RAM allocated for each replicas.                                        |
||||
| WORKERS_PREVIEW_PDF_TIMEOUT    | 600                        | Timeout for a PDF preview process.                                                  |
| WORKERS_PREVIEW_PDF_REPLICA    | 1                          | Number of replicas deployed on *each* worker server.                                |
| WORKERS_PREVIEW_PDF_CPU        | 1                          | Number of CPUs allocated for each replicas.                                         |
| WORKERS_PREVIEW_PDF_MEMORY     | 4g                         | Quantity of RAM allocated for each replicas.                                        |
||||
| WORKERS_SVG_TIMEOUT            | 7200                       | Timeout for a SVG process.                                                          |
| WORKERS_SVG_REPLICA            | 1                          | Number of replicas deployed on *each* worker server.                                |
| WORKERS_SVG_CPU                | 1                          | Number of CPUs allocated for each replicas.                                         |
| WORKERS_SVG_MEMORY             | 8g                         | Quantity of RAM allocated for each replicas.                                        |
||||
| WORKERS_XKT_TIMEOUT            | 600                        | Timeout for a XKT v6 process.                                                       |
| WORKERS_XKT_REPLICA            | 1                          | Number of replicas deployed on *each* worker server.                                |
| WORKERS_XKT_CPU                | 1                          | Number of CPUs allocated for each replicas.                                         |
| WORKERS_XKT_MEMORY             | 4g                         | Quantity of RAM allocated for each replicas.                                        |
||||
| WORKERS_XKT_V10_TIMEOUT        | 600                        | Timeout for a XTK v10 process.                                                      |
| WORKERS_XKT_V10_REPLICA        | 1                          | Number of replicas deployed on *each* worker server.                                |
| WORKERS_XKT_V10_CPU            | 1                          | Number of CPUs allocated for each replicas.                                         |
| WORKERS_XKT_V10_MEMORY         | 4g                         | Quantity of RAM allocated for each replicas.                                        |
