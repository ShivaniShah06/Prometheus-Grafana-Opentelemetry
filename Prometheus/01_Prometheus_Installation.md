# Install Prometheus

## Installing on Ubuntu
1. Spin up an EC2 ubuntu server and `add port 9090 in its security group` to be able to access Prometheus from this port

2. Get the Prometheus binary link from its documentation: https://prometheus.io/download/, copy the bindary URL, and run:

   ```shell
   $ sudo apt-get update
   $ sudo wget <download_url>
   ```

3. Add user and group for Prometheus:
   ```shell
   $ sudo groupadd --system prometheus
   $ sudo useradd -s /sbin/nologin --system -g prometheus prometheus
   ```

4. Make directories:
   ```shell
   $ mkdir /var/lib/prometheus
   $ mkdir -p /etc/prometheus/rules
   $ mkdir -p /etc/prometheus/rules.s
   $ mkdir -p /etc/prometheus/files_sd
   ```

5. Untar the downloaded binary:
   ```shell
   $ sudo untar -xvzf <binary_name>

   ```
6. Go in the new folder that got created after the untar and move files and folders:
   ```shell
   $ sudo mv prometheus /usr/local/bin
   # Try running the version command to see if its working. If not, then check if the correct binary was downloaded
   $ prometheus --version 
   $ sudo mv prometheus.yml /etc/prometheus/prometheus.yml
   ```

7. Create a service file for prometheus (This allows prometheus to automatically start at boot):
   ```shell
   $ sudo vi /etc/systemd/system/prometheus.service
   # The content should be as below:
   [Unit]
   Description=Prometheus
   Documentation=https://prometheus.io/docs/introduction/overview/
   Wants=network-online.target
   After=network-online.target

   [Service]
   Type=simple
   User=prometheus
   Group=prometheus
   ExecReload=/bin/kill -HUP $MAINPID
   ExecStart=/usr/local/bin/prometheus \
    --config.file=/etc/prometheus/prometheus.yml \
    --storage.tsdb.path=/var/lib/prometheus \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries \
    --web.listen-address=0.0.0.0:9090 \
    --web.external-url=

   SyslogIdentifier=prometheus
   Restart=always

   [Install]
   WantedBy=multi-user.target
   ```

8. Change folder ownerships and permissions:
   ```shell
   $ sudo chown -R prometheus:prometheus /etc/prometheus/
   $ sudo chown -R prometheus:prometheus /var/lib/prometheus/
   $ sudo chmod -R 775 /etc/prometheus/
   ```
9. Enable and start the service:
   ```shell
   $ sudo systemctl daemon-reload
   $ sudo systemctl start prometheus
   $ sudo systemctl enable prometheus
   $ sudo systemctl status prometheus
   ```

10. Go to `http://<public-ip>:9090` from a web browser and you should see Prometheus UI
