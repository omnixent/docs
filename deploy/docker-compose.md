# Deploy with Docker

Omnixent includes everything you need for deploying your instance to production using Docker and docker-compose.
All you need is a server with Docker installed and the following files:

- **docker-compose.yml**
- **Caddyfile**

By default, Omnixent ships with [Caddy](https://caddyserver.com) as a pre-configured reverse proxy, but you can use whatever you prefer (NGINX, HAProxy, etc.).

Let's open `docker-compose.yml` and add the following content:

```yaml
version: '3.7'

services:

  # The Omnixent container
  node:
    container_name: omnixent-node
    # Make sure that this is the latest version on DockerHub
    image: omnixent/omnixent:v0.0.43 
    ports:
      - 3000:3000
    restart: unless-stopped
    links:
      - redis
    environment:
      - NODE_ENV=production
      - REDIS_ENABLED=true
      - REDIS_URL=redis://redis:6379
      - ENABLE_PUBLIC_API=true
      - PORT=3000
      - DEFAULT_CACHE_EXPIRE_TIME=86400000
      # CHANGE THIS KEY BEFORE STARTING DOCKER-COMPOSE!
      - OMNIXENT_JWT_SECRET={"type":"HS256","key":"cKKgafNNj7vPsA4tDyqB8r9WXpEZPfru"} 
    networks:
      - caddynet

  caddy:
    image: abiosoft/caddy
    container_name: omnixent-caddy
    cap_add:
      - CAP_NET_BIND_SERVICE
    ports:
      - 9010:9010
      - 9011:9011
    expose:
      - 9010
      - 9011
    links:
      - node
    depends_on:
      - node
    volumes:
      - ./Caddyfile:/etc/Caddyfile
    command: -conf /etc/Caddyfile
    environment:
      CA_URL: https://acme-staging-v02.api.letsencrypt.org/directory
    restart: always
    networks:
      - caddynet

  redis:
    image: 'redis:alpine'
    container_name: omnixent-cache
    expose:
      - 6379
    networks:
      - caddynet

networks:
  caddynet:
    driver: bridge
```

The `docker-compose.yml` file above will install the following containers:

- **Redis**, to be used as a cache for your queries.
- **Caddy**, to be used as a reverse proxy for exposing Omnixent on ports 80 and 443. It also provides a free SSL certificate for your domains.
- **Omnixent**

Now let's open the file `Caddyfile` and add the following content:

```
yourdomain.com:80, yourdomain.com:443 {
  gzip
  cors
  log stdout
  errors stdout

  proxy / omnixent-node:3000 {
    transparent
  }

  header / Access-Control-Allow-Headers x-omnixent-auth
}
```

Make sure to replace `yourdomain.com` with your actual domain name.

# Upgrading Omnixent via docker-compose

Thanks to `docker-compose`, you can hot-upgrade your Omnixent version by upgrading the Omnixent image version inside of `docker-compose.yml` and then run:

```
$ docker-compose pull
$ docker-compose up -d
```