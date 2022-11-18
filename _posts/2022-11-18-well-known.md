---
title: "Co-hosting vanity Matrix and Mastodon servers"
categories:
  - Blog
tags:
  - computers
#header:
#  image: /assets/images/2022-07-06-calcinus-header.jpg
#  caption: Calcinus dapsiles
---

Quite a few years ago I met a [travel agent whose email address was `tr@vel.com.au`](https://www.globallink.com.au/contact-us/).

He inspired me to look at interesting domains and, to my great delight, American Samoa let me register tthi.as - resulting in my website at `https://ma.tthi.as' and my email address of `m@tthi.as`.

More recently I've become interested in open and federated social networks, resulting in the great usernames of `ma:tthi.as` and `@m@tthi.as` on Matrix and Mastodon, respectively. I'm running a server for each in Docker on a server in my store room.

Both Synapse (the "default" Matrix server) and Mastodon let you run a server at a subdomain, but both require the root domain to point to the correct subdomain. Matrix wants something served from `http://{domainurl}/.well-known/matrix` and Mastodon looks at `http://{domainurl}/.well-known/webfinger`.

Since I already run everything in containers behind Traefik, I decided to use a small Nginx container to serve this content. First up, my `docker-compose.yml`:

```
version: '3'

services:
  web:
    restart: unless-stopped
    image: nginx:alpine
    networks:
      - web
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
      - ./content:/var/www/
    environment:
      - TZ=Australia/Perth
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.wellknown.entrypoints=websecure"
      - "traefik.http.routers.wellknown.rule=(Host(`tthi.as`))"
      - "traefik.http.routers.wellknown.service=wellknown"
      - "traefik.http.routers.matrix.tls.certresolver=letsencrypt"
      - 'traefik.http.routers.matrix.tls.domains[0].main=tthi.as'
      - "traefik.http.services.wellknown.loadbalancer.server.port=80"
      - "traefik.docker.network=web"

networks:
  web:
    external: true
```

Secondly, a bare-bones `nginx.conf`. It serves the Matrix content from static files on the local filesystem, but redirects Mastodon requests to my Mastodon server.

```
events {
    use epoll;
    worker_connections 128;
}

http {
  server {
    server_name tthi.as;
    listen 0.0.0.0:80;

    location / {
      root /var/www;
      index index.html;
    }
    location /.well-known/webfinger {
      return 301 https://social.tthi.as$request_uri;
    }
  }
}
```

So far, so good. Nothing broken!