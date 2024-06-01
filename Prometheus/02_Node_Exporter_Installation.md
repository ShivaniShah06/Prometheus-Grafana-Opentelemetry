# Install Node Exporter

## Installing on Ubuntu application server
1. Spin up an EC2 server with inbound rule for `port 9100` and source as the security group of the Prometheus server
2. Get the Node Exporter binary link from its documentation: https://prometheus.io/download/, copy the bindary URL, and run:
   ```shell
   $ sudo apt-get update
   $ sudo apt-get upgrade
   $ sudo wget <download_url>
   ```
3. Add user and group for Prometheus:
   ```shell
   $ sudo groupadd --system prometheus
   $ sudo useradd -s /sbin/nologin --system -g prometheus prometheus
   ```

4. Make directory:
   ```shell
   $ sudo mkdir /var/lib/node/
   ```

5. Move file to directory:
   ```shell
   # Go inside the folder that was created after you untarred the binary for node exporter
   $ sudo mv node_exporter /var/lib/node/
   ```

6. Add service for node exporter (This allows node exporter to automatically start at boot):
   ```shell
   $ sudo vi /etc/systemd/system/node.service
   # service file content
   [Unit]
   Description=Prometheus Node Exporter
   Documentation=https://prometheus.io/docs/introduction/overview/
   Wants=network-online.target
   After=network-online.target

   [Service]
   Type=simple
   User=prometheus
   Group=prometheus
   ExecReload=/bin/kill -HUP $MAINPID
   ExecStart=/var/lib/node/node_exporter

   SyslogIdentifier=prometheus_node_exporter
   Restart=always

   [Install]
   WantedBy=multi-user.target
   ```

7. Change folder ownerships and permissions:
   ```shell
   $ sudo chown -R prometheus:prometheus /var/lib/node/
   $ sudo chmod -R 775 /var/lib/node/
   ```

8. Enable and start the service:
   ```shell
   $ sudo systemctl daemon-reload
   $ sudo systemctl start node
   $ sudo systemctl enable node
   $ sudo systemctl status node
   ```