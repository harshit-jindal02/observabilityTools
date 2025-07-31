# Grafana Loki: An Introduction

![Loki Logo](https://grafana.com/static/assets/img/loki-logo-horizontal-dark-background.svg)

Welcome to Grafana Loki! If you're looking for a simple, scalable, and cost-effective log aggregation system, this guide is for you. It explains what Loki is, its unique design philosophy, and how it fits perfectly into the Grafana observability stack.

## What is Loki?

**Grafana Loki is a horizontally scalable, highly available, multi-tenant log aggregation system inspired by Prometheus.**

The core idea behind Loki is to be very cost-effective and easy to operate. Unlike other logging systems that index the full content of your logs (full-text indexing), Loki takes a different approach. It only indexes a small set of metadata about your logs, called **labels**. The log data itself is compressed and stored in object stores like S3 or GCS.

This makes Loki incredibly efficient but still allows you to query and analyze your logs effectively, especially when combined with metrics from Prometheus and visualizations in Grafana.

## Core Concepts: The Loki Approach

Loki's design is intentionally simple and built on three key concepts:

### 1. Labels
This is the most important concept in Loki. Instead of indexing the full text of a log message, Loki groups log streams using a set of key-value pairs called **labels**. These are the same labels you would use in Prometheus for your metrics (e.g., `app="api"`, `cluster="us-central-1"`). This consistency makes correlating metrics and logs seamless.

### 2. Log Streams
A **log stream** is a unique set of labels. All logs that share the exact same set of labels belong to the same stream. Within a stream, logs are stored in chronological order. This design makes writing logs very efficient, as Loki just has to find the correct stream (based on labels) and append the new log line.

### 3. LogQL (Log Query Language)
LogQL is Loki's query language, which is heavily inspired by Prometheus's PromQL. You use LogQL in Grafana to query your logs. A query typically has two parts:
* **A stream selector:** This part uses labels to select which log streams to search (e.g., `{app="api", namespace="production"}`).
* **A log filter:** An optional part that can perform keyword searches or regular expression matching on the raw log message of the selected streams (e.g., `|= "error"` or `|~ "status=5.."`).

## Why Use Loki?

* **Cost-Effective:** By indexing only metadata, Loki dramatically reduces storage and memory costs compared to systems that perform full-text indexing.
* **Easy to Operate:** The architecture is simpler, with fewer components to manage, making it easier to scale and maintain.
* **Seamless Correlation:** Designed to work perfectly with Prometheus and Grafana. You can effortlessly switch between metrics and the logs that correspond to them in a single Grafana dashboard.
* **Fast Ingestion:** The label-based approach makes ingesting logs extremely fast and efficient.

## Key Components of the Loki Stack

1.  **Promtail:** This is the agent that collects logs. You install Promtail on your servers or as a DaemonSet in Kubernetes. It discovers log files, attaches the correct labels (e.g., by extracting them from Kubernetes pod labels), and pushes the log streams to the Loki server.
2.  **Loki:** This is the main server that receives log streams from Promtail, indexes the labels, and stores the compressed log data.
3.  **Grafana:** This is the primary user interface for Loki. You add Loki as a data source in Grafana, and then you can use the "Explore" view to query, visualize, and alert on your logs using LogQL.

## Getting Started: A Conceptual Example

Setting up a Loki logging pipeline is straightforward:

1.  **Run Loki:** Start the Loki server. For testing, it can be run as a single binary or Docker container.
2.  **Run Promtail:** Deploy the Promtail agent to the machine where your application is running.
3.  **Configure Promtail:** Create a configuration file for Promtail that tells it where to find log files and how to label the streams. For example, it can automatically discover Docker container logs and use the container's labels as Loki labels.
4.  **Add Loki to Grafana:** In the Grafana UI, go to "Data Sources" and add Loki, pointing it to your Loki server's URL.
5.  **Explore Logs:** Go to Grafana's "Explore" view, select your Loki data source, and start writing LogQL queries to find your logs (e.g., `{job="mysql"}`).

## Further Reading & Community

* **Official Website:** [https://grafana.com/oss/loki/](https://grafana.com/oss/loki/)
* **Official Documentation:** [https://grafana.com/docs/loki/latest/](https://grafana.com/docs/loki/latest/)
* **Loki on GitHub:** [https://github.com/grafana/loki](https://github.com/grafana/loki)

---
We hope this guide provides a clear understanding of Loki's powerful and efficient approach to log aggregation!