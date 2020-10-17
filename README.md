# bitwarden_rs-docker-setup

A production grade, Dockerized bitwarden_rs setup with NGINX, Certbot, and Fail2ban included.

## Getting Started

1. Clone repo to your server.
2. Create `.env` files in `bitwarden` and `fail2ban` folders.
3. Populate `.env` files using the provide `template.env` files.
4. Adjust volume source paths in all `docker-compose.yml` files.
5. Edit `default.conf` for NGINX to correctly represent your domain(s). Be sure to remove any unwanted server declarations.
6. Review the fail2ban settings under `fail2ban/data/jail.d/bitwarden.local`. Modify as desired.
6. Start the containers using `docker-compose up -d` in the following order: bitwarden, fail2ban, nginx-certbot.

## Adding HTTPS Certs

Ensure your DNS is already updated with an `A` record pointed to your server's IP.

1. Locate container ID for NGINX using `docker ps -a`.
2. Run `docker exec -it CONTAINER_ID /bin/bash`. You should now be inside the container as root.
3. Run `certbot -d domain.com` to generate a cert for domain.com. Follow on-screen instructions to complete process. When asked, allow certbot to automatically update the NGINX config with new cert info.
4. `exit` from the container.
4. Done. If needed, you can run `docker-compose restart` in the `nginx-certbot` folder to restart NGINX with the updated config.

## Hardening Tips

- Add automated backups of your bitwarden_rs database using other projects like:
  - https://github.com/shivpatel/bitwarden_rs-local-backup
  - https://github.com/shivpatel/bitwarden_rs_dropbox_backup
- Ensure your Bitwarden homepage cannot be accessed via the server's IP (e.g. https://IP). IPs can be easily scanned making your bitwarden_rs instance a more obvious target if it can be detected this way.
- Use a firewall for your VM.
   - Deny all for SSH except your IP address.
   - If possible and feasible for your use case, only allow HTTP/HTTPS to your IP address.
- If you're still using SSH login via password, reconsider hosting your own password manager :)