# Prometheus: An Introduction

![Prometheus Logo](https://prometheus.io/assets/prometheus_logo_grey.svg)

Welcome to Prometheus! If you're looking for a powerful, open-source monitoring and alerting toolkit designed for reliability and scalability, this guide is for you. It covers what Prometheus is, its core data model, and how its components work together to provide deep insight into your systems.

## What is Prometheus?

**Prometheus is an open-source systems monitoring and alerting toolkit** originally built at SoundCloud. Since its inception, it has become a graduated project of the Cloud Native Computing Foundation (CNCF) and is now the de-facto standard for metrics-based monitoring in the cloud-native world.

At its core, Prometheus collects and stores its metrics as time-series data, i.e., metrics information is stored with the timestamp at which it was recorded, alongside optional key-value pairs called labels. Its defining characteristic is its **pull-based model**, where it periodically scrapes (fetches) metrics from configured endpoints on the systems it monitors.

## Core Concepts: The Prometheus Way

Prometheus has a powerful and specific model for handling data.

### 1. Metrics and Labels (Dimensional Data Model)
All data in Prometheus is a **time series** identified by a metric name and a set of key-value pairs called **labels**. This dimensional data model allows for powerful filtering, grouping, and querying. For example, a metric `http_requests_total` can have labels like `method="POST"` and `handler="/api/users"` to slice and dice the data.

### 2. Scraping (Pull Model)
Prometheus discovers and collects metrics by periodically sending an HTTP request to an endpoint on a target system. These targets expose their current state as a set of metrics in a simple text-based format. This pull model simplifies configuration and makes it easy to know which systems are being monitored and whether they are healthy.

### 3. PromQL (Prometheus Query Language)
Prometheus features a powerful functional query language called **PromQL**. You use PromQL to select and aggregate time-series data in real-time. It can be used for everything from creating graphs and dashboards to defining alerting rules.

### 4. Alerting
Alerting rules are defined directly in Prometheus using PromQL. The Prometheus server continuously evaluates these rules. If an alert's condition is met, it fires the alert and sends it to a separate component, the **Alertmanager**, for handling.

## Why Use Prometheus?

* **Powerful Queries:** PromQL is an incredibly flexible language for analyzing your monitoring data.
* **Dimensional Data:** The label-based data model is a powerful way to model and query multi-dimensional metrics.
* **Reliability:** Prometheus is designed to be reliable and is the system you turn to during an outage to diagnose problems. Each server is standalone, not depending on network storage or other remote services.
* **Service Discovery:** It can automatically discover targets to monitor via integrations with Kubernetes, AWS, and other platforms.
* **Open Source & Huge Ecosystem:** It has a massive community and integrates with hundreds of services via "exporters."

## Key Components of the Prometheus Ecosystem

1.  **Prometheus Server:** The main component that scrapes metrics from targets, stores the time-series data, and evaluates alerting rules.
2.  **Client Libraries:** For instrumenting your own application code. You add a client library to your code to create metrics and expose them via an HTTP endpoint for the Prometheus server to scrape.
3.  **Exporters:** For services you can't directly instrument (like a database, a Linux host, or a hardware load balancer). Exporters are standalone services that act as a proxy, fetching stats from the target system and converting them into the Prometheus text format.
4.  **Alertmanager:** Handles alerts sent by Prometheus. It takes care of de-duplicating, grouping, and routing them to the correct receiver (like email, PagerDuty, or Slack). It also handles silencing and inhibition of alerts.

## Getting Started: A Conceptual Example

1.  **Run Prometheus:** Download and run the Prometheus server.
2.  **Configure a Target:** Edit the Prometheus configuration file (`prometheus.yml`) to tell it where to find targets to scrape. By default, it's configured to scrape its own metrics.
3.  **Run an Exporter:** Download and run an exporter for a service you want to monitor, like the Node Exporter for Linux system metrics.
4.  **Add the Exporter as a Target:** Add the address of the Node Exporter to your `prometheus.yml` configuration and restart Prometheus.
5.  **Query Data:** Open the Prometheus UI in your browser (at `http://localhost:9090`). You can now use the expression browser to explore the collected metrics and write PromQL queries.

## Further Reading & Community

* **Official Website:** [https://prometheus.io/](https://prometheus.io/)
* **Official Documentation:** [https://prometheus.io/docs/introduction/overview/](https://prometheus.io/docs/introduction/overview/)
* **Prometheus on GitHub:** [https://github.com/prometheus/prometheus](https://github.com/prometheus/prometheus)

---
We hope this guide provides a solid foundation for understanding the power and architecture of Prometheus!