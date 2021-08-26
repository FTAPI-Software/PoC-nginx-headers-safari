# PoC nginx conditional headers
This project provides a proof of concept for unsetting HTTP-Headers based on the User-Agent using nginx

## Prerequisites
1. Install [Docker](https://docs.docker.com/engine/install/)
1. Install [Docker-Compose](https://docs.docker.com/compose/install/)

## Usage
1. Start the containers: `docker-compose up --build --force-recreate -d`
1. Use curl and look for the X-Frame-Options header:
    1.1. `curl -v -k -A "Firefox" https://localhost:8443` *header should appear*
    1.1. `curl -v -k -A "Safari" https://localhost:8443` *header should **not** appear*
    1.1. `curl -v -k -A "Safari: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/14.0 Safari/605.1.15" https://localhost:8443` *header should **not** appear*
1. Teadown the containers: `docker-compose down`

## Inner Works
This PoC uses 2 containers:
1. **http-service:** a simple http server that serves a page on port 80. For the purpose of this PoC it always sets the X-Frame-Options header.
1. **revproxy:** An nginx reverse proxy that terminates TLS and sends http traffic off to **http-service**.

**revproxy** uses the [Header More nginx module](https://github.com/openresty/headers-more-nginx-module#readme), speficially the **more_clear_headers** directive, in combination with the
default **if** directive, to remove the X-Frame-Options header when the User-Agent contains "Safari".
