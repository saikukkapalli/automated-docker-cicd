# Docker Compose + Nginx HTTPS Practice

## Architecture
- Docker Compose runs the application and Nginx containers.
- Nginx acts as a reverse proxy.
- The application is available internally on the Docker network.
- Host port 8080 is used for HTTP testing.
- Host port 8443 is used for HTTPS testing.

## Prerequisites
- Docker Engine
- Docker Compose
- OpenSSL

## Generate a self-signed certificate

Run from this directory:

    mkdir -p certs

    openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
      -keyout certs/privkey.pem \
      -out certs/fullchain.pem \
      -subj "/CN=localhost"

## Start the services

    sudo docker compose config
    sudo docker compose up -d
    sudo docker compose ps

## Test HTTPS

    curl -k -I https://localhost:8443

The -k flag is for testing with the self-signed certificate only.

## Security notes
- Never commit private keys or certificates to Git.
- The self-signed certificate is not publicly trusted.
- For production, use a publicly trusted certificate and a domain
  or supported IP-address certificate configuration.
- Restrict test ports in the EC2 security group.
