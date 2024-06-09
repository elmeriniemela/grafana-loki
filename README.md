#### Grafana loki/prometheus monitoring

* git clone git@github.com:elmeriniemela/grafana-loki.git /opt/grafana-loki
* docker install: https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository
* apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
* apt install nginx
* ln -s /opt/grafana-loki/nginx/* /etc/nginx/sites-enabled/
* cd /etc/ssl && openssl req -nodes -x509 -new -keyout private/nginx-selfsigned.key -out certs/nginx-selfsigned.crt -days 3650
* systemctl reload nginx
* Start: docker compose up -d --force-recreate
* Stop: docker compose down

