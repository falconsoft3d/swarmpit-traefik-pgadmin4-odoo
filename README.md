# swarmpit-traefik-pgadmin4-odoo
```
version: '3.3'
services:
  odoo16:
    image: odoo:16
    environment:
      HOST: postgres_postgres13
      PASSWORD: odoo
      PORT: '5432'
      USER: odoo
    volumes:
     - data:/var/lib/odoo/
    networks:
     - postgres_app-postgree
     - traefik
    logging:
      driver: json-file
    deploy:
      labels:
        traefik.http.routers.odoo16-https.entrypoints: https
        traefik.http.routers.odoo16-https.tls.certresolver: letsencrypt
        traefik.http.routers.odoo16-http.middlewares: odoo16-https-redirect
        swarmpit.service.deployment.autoredeploy: 'true'
        traefik.http.routers.odoo-16-http.entrypoints: http
        traefik.http.middlewares.odoo16-https-redirect.redirectscheme.scheme: https
        traefik.http.routers.odoo16-https.rule: Host(`odoo16.odooerp.online`)
        traefik.http.routers.odoo16-http.rule: Host(`odoo16.odooerp.online`)
        traefik.http.routers.odoo16-https.service: odoo16
        traefik.http.routers.odoo16-https.tls: 'true'
        traefik.http.routers.odoo16-http.service: odoo16
        traefik.enable: 'true'
        traefik.http.services.odoo16.loadbalancer.server.port: '8069'
      update_config:
        delay: 2s
networks:
  postgres_app-postgree:
    external: true
  traefik:
    external: true
volumes:
  data:
    driver: local
    ```
