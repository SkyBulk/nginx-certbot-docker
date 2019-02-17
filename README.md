# nginx-certbot-docker
Dockerized NGINX with Certbot pre-installed and auto cert renewals.

## Setup
1. Clone repository and ensure Docker and docker-compose is installed.
2. Replace contents of `default` with your current NGINX setup. If you're already using SSL, only migrate basic HTTP/80 configs; makes it easier for Certbot to auto-edit this file later on.
3. Run `docker-compose build`
4. Run `docker-compose up -d` to run the image in daemon mode.
5. Run `docker ps -a` to locate the container ID for our new container.
6. Run `docker exec -it CONTAINER_ID /bin/bash`
7. Run `certbot --nginx` and follow the guided instructions to automatically create certs and apply them to your NGINX config.
8. You can run any other comands at this step while you're still in the container.
9. Run `exit` to quit out of your container bash session.
10. Run `docker-compose restart` to restart NGINX with our certbot-edited NGINX configs.
11. Done! See if your sites are up and accessible. Your container will auto restart when it fails or you restart the server. Auto cert renewals are enabled by default and no further action needs to be taken!

## Tips
- To manually make changes to your NGINX default config, you'll need to `docker-compose restart`.
- You may need to volume mount static file directories from the host. You can add this to the `volumes` section in `docker-compose.yml`.
- All your Let's Encrypt files will be stored in `./etc/letsencrypt` relative to this repo's folder.
- If you're running your actual web service (e.g. Node Express, Python Flask, etc.) on a seperate container or on the host, you'll need to explore NGINX proxying and Docker networking to allow communication across containers.