# Uptime Kuma

Uptime Kuma is the first application deployed on VoomLab.
 
It provides a lightweight web interface for monitoring services and endpoints running on the homelab.

## Deployment

Uptime Kuma runs as a Docker container using Docker Compose.

Deployment directory:

```text
docker/uptime-kuma/
└── compose.yml

Start the service:
docker compose -f compose.yml up -d

Stop the service:
docker compose -f compose.yml down

View logs:
docker compose -f compose.yml logs -f

Configuration

Uptime Kuma listens on port 3001.

The application data is stored in a Docker named volume:
uptime-kuma-data

This keeps the application data persistent across container recreation.

Why Uptime Kuma?

Uptime Kuma was selected as the first VoomLab service because it is:

* Lightweight
* Self-hosted
* Useful for monitoring infrastructure
* Easy to deploy with Docker
* Suitable for a low-resource machine

Resource Considerations

VoomLab has only 4 GB of RAM.

For this reason, services are intentionally kept lightweight and additional infrastructure will only be deployed when there is a clear purpose.

Future Improvements

Possible future improvements include:

* Healthchecks
* Container resource limits
* Centralized logging
* Backup strategy
* Monitoring additional VoomLab services
