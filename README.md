# Docker Compose Media Server and Automation Setup

This Docker Compose configuration sets up various containers for managing and automating services related to media streaming, torrenting, and network management. The setup includes applications for DNS filtering, media management, downloading, reverse proxying, and system automation.

The services are connected via a custom Docker network (`omv`), ensuring each container can communicate with others on a dedicated subnet. The containers are configured to persist important data, such as media libraries and configurations, and to expose relevant ports for accessing web interfaces.

## Features

### DNS Filtering & Ad Blocking:
- **AdGuard Home**: A DNS-based ad blocker for network-wide filtering. It also supports DNS rewrites for local services, meaning you don't need to use Cloudflare or external DNS for internal network services. This is useful when accessing internal services via their domain names (e.g., `emby.local`).

### Media Management:
- **Emby**: A media server to organize and stream movies and TV shows.
- **Kavita**: A service for organizing and reading eBook libraries.
- **Radarr**: Automatically downloads and organizes movies, and manages your movie library.
- **Sonarr**: Automatically downloads, organizes, and manages your TV shows and anime library.
- **Prowlarr**: Indexer manager for NZB and torrent clients like Radarr and Sonarr.

### Torrent Management:
- **qBittorrent**: A torrent client for downloading media files with a web UI for management.
- **Flaresolverr**: A service to bypass Cloudflare protection, allowing seamless integration with various APIs.

### Reverse Proxy & SSL:
- **SWAG (Secure Web Application Gateway)**: A reverse proxy service that handles SSL termination, DNS validation (via Cloudflare), and access to internal services via subdomains.

### Automation & Monitoring:
- **Watchtower**: Automatically updates containers when new versions are released and notifies you of the updates.

### Cloudflare Tunnel:
- **Cloudflare**: A service to securely expose local services to the internet using Cloudflare's tunnel.

## Network Configuration

The services are connected through a custom bridge network (`omv`) with a dedicated subnet (`10.5.0.0/24`). This setup allows containers to communicate with each other using static IP addresses within the subnet, while also enabling secure communication with external services.

## Persistent Storage

Each application that requires persistent data is configured with volumes to ensure that data is retained across container restarts. These volumes include:

- **Media Volumes**: For storing media files such as movies, TV shows, and eBooks.
- **Config Volumes**: For storing and preserving application configurations and settings.

These volumes are bound to host directories, allowing easy access and management of files.

## Ports

Several ports are exposed for external access:

- **AdGuard Home**: Exposes DNS port (53/UDP).
- **qBittorrent**: Exposes ports for web UI and torrenting traffic.
- **SWAG**: Exposes HTTP/HTTPS ports for reverse proxy and SSL traffic.
- **Emby, Radarr, Sonarr, Prowlarr**: Exposes web UI ports for accessing the respective applications.

## Services Overview

| Service         | Description                                                                                      | Static IP Address |
|-----------------|--------------------------------------------------------------------------------------------------|-------------------|
| **AdGuard**     | DNS-based ad blocker and network-wide filtering, also allows for DNS rewrites for local services | 10.5.0.101        |
| **Cloudflare** | Cloudflare tunnel service for secure access                                                     | 10.5.0.102        |
| **Emby**        | Media server for organizing and streaming media                                                 | Host mode         |
| **Organizr**    | Web interface for organizing media apps                                                         | 10.5.0.104        |
| **Kavita**      | eBook management and library service                                                              | 10.5.0.105        |
| **Prowlarr**    | Indexer manager for NZB and torrent clients                                                      | 10.5.0.106        |
| **qBittorrent** | Torrent client for media downloads                                                                | 10.5.0.107        |
| **Radarr**      | Movie downloader and organizer, manages the movie library                                        | 10.5.0.108        |
| **Sonarr**      | TV show downloader and organizer, manages the TV show library                                    | 10.5.0.109        |
| **SWAG**        | Reverse proxy with SSL support                                                                   | 10.5.0.110        |
| **Flaresolverr**| Service for bypassing Cloudflare protection                                                      | 10.5.0.111        |
| **Watchtower**  | Automatic container updater and notifier                                                         | 10.5.0.112        |

## Volumes

The configuration uses Docker volumes for persistent storage of media, downloads, and application data. These volumes are bound to directories on the host system:

- **Media Volume**: Stores media files like movies and TV shows.
- **Downloads Volume**: Stores downloaded content.
- **Users Volume**: Stores user data.

## How to Use

This guide provides step-by-step instructions for setting up and running a Docker Compose-based media server and automation configuration.

### Prerequisites

Before proceeding, ensure the following software is installed on your system:

- **Docker**: Required to run the containers.
- **Docker Compose**: Manages multi-container Docker applications.
- **Git** (optional): For cloning the repository directly.

If Docker or Docker Compose is not installed, follow the official installation instructions:
- [Install Docker](https://docs.docker.com/get-docker/)
- [Install Docker Compose](https://docs.docker.com/compose/install/)

### Step 1: Clone the Repository

You can either clone the repository or manually download the `docker-compose.yml` file to your local machine.

1. Clone the repository with the following command or manually download the `docker-compose.yml` file:
    ```bash
    git clone https://github.com/TropicSatern36/OMV_Compose.git .
    ```

2. **Edit the `.env` file** to configure your environment settings as needed.

3. Run the following command to start all services in detached mode:
    ```bash
    docker-compose up -d
    ```

This will initialize the services defined in the `docker-compose.yml` file. The `-d` flag ensures that the containers run in the background.

### Additional Notes
- Make sure to adjust any volume mappings and environment variables in the `.env` file based on your specific requirements.
- If you encounter issues, refer to the Docker logs using `docker-compose logs` for troubleshooting.

## Notes

- The configuration is designed for use with a local network and uses static IP addresses within the `10.5.0.0/24` subnet.
- Ensure that your firewall is configured to allow traffic on the necessary ports.
- The Watchtower service automatically updates containers and sends notifications upon completion.
- The SWAG service handles SSL certificates through Cloudflare DNS validation, ensuring secure access to your services.
- AdGuard Home allows you to rewrite DNS requests for local services, eliminating the need for external DNS (such as Cloudflare) when accessing internal services in your network.
