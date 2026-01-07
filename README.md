# Home Lab Infrastructure

This project provides a ready-to-use infrastructure-as-code setup for a home lab or a small server environment, managed using **Docker Compose**. It features automated SSL/TLS certificate management, a reverse proxy, and several management tools.

## Services

- **Traefik**: The edge router and reverse proxy. It handles SSL certificates automatically via Let's Encrypt (DNS-01 challenge with DigitalOcean).
- **Portainer**: A web-based management interface for Docker.
- **Dozzle**: A real-time log viewer for Docker containers.
- **Registry**: A private Docker image registry.
- **Watchtower**: Automatically updates running Docker containers when new images are pushed.

## Prerequisites

- Docker and Docker Compose installed.
- A domain managed by DigitalOcean (for the DNS-01 challenge).
- A DigitalOcean API token.

## Getting Started

1.  **Clone the repository:**
    ```bash
    git clone <repository-url>
    cd lab
    ```

2.  **Configure environment variables:**
    Copy the example environment file and edit it with your details:
    ```bash
    cp .env.example .env
    ```
    Edit `.env` and set the following variables:
    - `DOMAIN`: Your base domain (e.g., `example.com`).
    - `ACME_EMAIL`: Your email address for Let's Encrypt notifications.
    - `DO_AUTH_TOKEN`: Your DigitalOcean API token.

3.  **Prepare the proxy network:**
    The services communicate over an external Docker network named `proxy`. Create it before starting the services:
    ```bash
    docker network create proxy
    ```

4.  **Launch the services:**
    Run the provided startup script:
    ```bash
    ./up
    ```

## Service Access

Once everything is up and running, you can access the services at the following subdomains:

- **Traefik Dashboard**: `https://control.<your-domain>`
- **Portainer**: `https://portainer.<your-domain>`
- **Dozzle (Logs)**: `https://logs.<your-domain>`
- **Docker Registry**: `https://hub.<your-domain>`

## Project Structure

- `traefik/`: Traefik configuration and certificate storage.
- `portainer/`: Portainer deployment and data persistence.
- `dozzle/`: Dozzle deployment.
- `registry/`: Private registry configuration and data.
- `watchtower/`: Automated update service.
- `up`: A helper script to start all services in the correct order.

## Security Note

The `.env` file contains sensitive information and is excluded from source control via `.gitignore`. Never commit your real `.env` file or any `acme.json` containing private keys.
