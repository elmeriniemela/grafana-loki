
## Grafana loki/prometheus monitoring

This repository provides a self-hosted observability stack built with Grafana, Loki, and Prometheus. It includes Docker Compose and Nginx setup steps to deploy the services, enable basic TLS, and manage startup/shutdown.

### What is Grafana?

Grafana is a leading open-source platform for data visualization, monitoring, and analysis, allowing users to query, visualize, and alert on metrics, logs, and traces from diverse source

### What is Grafana loki?

Grafana Loki is a set of open source components that can be composed into a fully featured logging stack.

### What is Prometheus?

Prometheus scrapes metrics from instrumented jobs, either directly or via an intermediary push gateway for short-lived jobs. It stores all scraped samples locally and runs rules over this data to either aggregate and record new time series from existing data or generate alerts. Grafana or other API consumers can be used to visualize the collected data.

### Deployment notes

* git clone git@github.com:elmeriniemela/grafana-loki.git /opt/grafana-loki
* docker install: https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository
* apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
* apt install nginx
* ln -s /opt/grafana-loki/nginx/* /etc/nginx/sites-enabled/
* cd /etc/ssl && openssl req -nodes -x509 -new -keyout private/nginx-selfsigned.key -out certs/nginx-selfsigned.crt -days 3650
* systemctl reload nginx
* Start: docker compose up -d --force-recreate
* Stop: docker compose down
* `cp daemon.json /etc/docker/daemon.json`
