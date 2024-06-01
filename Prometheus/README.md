# PROMETHEUS

## Collecting Metrics on Prometheus
- One way is to use python or java built-in libraries in your application code to send the metrics to Prometheus but in all cases, we do not have access to the application code
* ## Infrastructure with lots of different components:
  - If millions of applications push metrics to Prometheus server, it can end up exhausting the server
  - So one possible solution is to have a bash scriptto collect the data from the database, run on s regular basis using a cron job, and send the information to Prometheus. However, it is not a good solution because it is not scalable
  - So a better solution is that Prometheus goes and pulls the data from these systems. In order to do so, we need to install `exporter` on or sometimes next to the system from where `Prometheus is supposed to pull the data`

  * ## Scraping
    - The process of connecting to an exporter and pulling the metrics into Prometheus is called `Scraping`
    - Scraping can be configured in Prometheus config file. The default is `15 seconds`

* ## An application that wants to send the metric to Prometheus
   - We need something called as `Push Gateway`
   - `Push Gateway` is a part of Prometheus which acts as a temporary storage where application can send the data
   - It has a `built-in exporter` so Prometheus can also scrape the information from a push gateway

> [!IMPORTANT]
> Prometheus is always a pull time-series database. You can never push anything to it. It always pulls.
 
## Types of Exporters
1. Node Exporter:
   - Every UNIX-based kernel eg: any computer using unix based OS is called a node
   - Node Exporter is an official Prometheus exporter for collecting metrics that are exposed by Unix-based kernels eg: Linux and Ubuntu
   - Example of metrics are CPU, Disk, Memory, & Network I/O
   - Can be extended with pluggable metric collectors
   - See this for instructions on how to install `node exporter` on your application server