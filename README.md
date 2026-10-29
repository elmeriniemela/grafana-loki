
## Grafana loki/prometheus monitoring

This repository provides a self-hosted observability stack built with Grafana, Loki, and Prometheus. It includes Docker Compose and Nginx setup steps to deploy the services, enable basic TLS, and manage startup/shutdown.

### What is Grafana?

Grafana is a leading open-source platform for data visualization, monitoring, and analysis, allowing users to query, visualize, and alert on metrics, logs, and traces from diverse source

### What is Grafana loki?

Grafana Loki is a set of open source components that can be composed into a fully featured logging stack.

### What is Prometheus?

Prometheus scrapes metrics from instrumented jobs, either directly or via an intermediary push gateway for short-lived jobs. It stores all scraped samples locally and runs rules over this data to either aggregate and record new time series from existing data or generate alerts. Grafana or other API consumers can be used to visualize the collected data.

### Installation on a new server (Ubuntu)

Run as root.

1. Clone the repository:
   ```sh
   git clone git@github.com:elmeriniemela/grafana-loki.git /opt/grafana-loki
   chown -R 1000 /opt/grafana-loki/grafana
   ```
2. Install Docker from its apt repository (https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository):
   ```sh
   apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```
3. Limit container log sizes. Do this before starting any containers, because the limits only apply to containers created afterwards:
   ```sh
   cp /opt/grafana-loki/daemon.json /etc/docker/daemon.json
   systemctl restart docker
   ```
4. Install nginx with a self-signed certificate:
   ```sh
   apt install nginx
   ln -s /opt/grafana-loki/nginx/* /etc/nginx/sites-enabled/
   cd /etc/ssl && openssl req -nodes -x509 -new -keyout private/nginx-selfsigned.key -out certs/nginx-selfsigned.crt -days 3650
   systemctl reload nginx
   ```
5. Configure the firewall. Docker publishes Grafana, Loki and Prometheus on 127.0.0.1 only, so they are reachable only through nginx:
   ```sh
   ufw default deny incoming
   ufw default allow outgoing
   ufw allow OpenSSH
   ufw allow 443/tcp                                                # Grafana and Prometheus via nginx
   ufw allow from 10.0.0.0/24 to any port 3110 proto tcp            # Loki via nginx, for promtail agents on the private network
   ufw allow from 172.16.0.0/12 to any port 9100 proto tcp          # node_exporter, scraped by the Prometheus container
   ufw enable
   ```
6. Start the stack:
   ```sh
   cd /opt/grafana-loki && docker compose up -d
   ```

### Operation

* Start: `docker compose up -d --force-recreate`
* Stop: `docker compose down`
* Check that every container has log limits (should show `max-size:10m`): `docker ps -q | xargs docker inspect --format '{{.Name}} {{.HostConfig.LogConfig}}'`
