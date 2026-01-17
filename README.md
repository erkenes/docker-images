# Docker Images

All images are published to the **GitHub Container Registry** and **Docker Hub**.

The images marked with `(DHI)` are based on the [Docker Hardened Image (DHI)](https://www.docker.com/products/hardened-images/) program and provide additional security features.

---

## Available images

### PHP

[![PHP](https://github.com/erkenes/docker-images/actions/workflows/php.yml/badge.svg)](https://github.com/erkenes/docker-images/actions/workflows/php.yml)

- [ghcr.io/erkenes/php/8.1-fpm-alpine](./php/8.1-fpm-alpine/Dockerfile)
- [ghcr.io/erkenes/php/8.2-fpm-alpine](./php/8.2-fpm-alpine/Dockerfile)
- [ghcr.io/erkenes/php/8.3-fpm-alpine](./php/8.3-fpm-alpine/Dockerfile)
- [ghcr.io/erkenes/php/8.4-fpm-alpine](./php/8.4-fpm-alpine/Dockerfile)

### Nginx (DHI)

[![Nginx](https://github.com/erkenes/docker-images/actions/workflows/nginx.yml/badge.svg)](https://github.com/erkenes/docker-images/actions/workflows/nginx.yml)

* [ghcr.io/erkenes/nginx/1-alpine](./nginx/1-alpine/Dockerfile)
* [ghcr.io/erkenes/nginx/1-alpine-dev](./nginx/1-alpine-dev/Dockerfile)
