# Configure Prometheus to scrape the Metrics from Application server/s

1. Go to `/etc/prometheus/prometheus.yml` on the Prometheus server and add below under `scrape_configs` section:
    ```shell
    scrape_configs:
    # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
    - job_name: "<name>"

      # metrics_path defaults to '/metrics'
      # scheme defaults to 'http'.

      static_configs:
        - targets: ["<application_server_public_ip>:9100"]
    ```

2. Restart the Prometheus service:
   ```shell
   $ sudo systemctl restart prometheus
   ```

3. Go to `Prometheus UI > Status > Targets` and you should be able to see the server as one of the targets and its `State` should be `UP`. If not:
   - Try checking ip address in security group and firewall connections
   - Try running `curl "http://<public_ip>:9100/metrics"` on Prometheus backend and see if you get metrics
   - If there is error `Get "http://<public_ip>:9100/metrics”: context deadline exceeded"`, try modifying `scrape_interval` and `scrape_timeout` for that particular job