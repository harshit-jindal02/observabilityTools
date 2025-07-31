# Chaos Monkey: An Introduction

![Chaos Monkey Logo](https://raw.githubusercontent.com/Netflix/chaosmonkey/master/chaosmonkey-core/src/main/resources/public/img/monkey.png)

Welcome to the world of Chaos Monkey! If you've heard about "Chaos Engineering" and want to understand the tool that started it all, you're in the right place. This guide will introduce you to the purpose, concepts, and value of Chaos Monkey.

## What is Chaos Monkey?

**Chaos Monkey is a resilience tool that helps applications tolerate random instance failures.** It was created by Netflix to test the stability of its infrastructure as it migrated to the AWS cloud.

The core idea is simple but powerful: Chaos Monkey intentionally and randomly terminates virtual machine instances and containers running in your production environment. If your system is resilient, it should survive these failures without any noticeable impact on the customer. This practice of intentionally injecting failure into a system to test its robustness is known as **Chaos Engineering**.

Chaos Monkey acts as a proactive way to expose weaknesses before they cause a real, widespread outage.

## Core Concepts: How Chaos Monkey Works

Chaos Monkey operates on a few simple but effective principles:

### 1. Integration with Spinnaker
Chaos Monkey is designed to integrate with **Spinnaker**, Netflix's continuous delivery platform. Spinnaker provides Chaos Monkey with the necessary information about your applications, clusters, and infrastructure, telling it which instances are eligible for termination.

### 2. Opt-Out Model
By default, all applications and clusters discovered via Spinnaker are considered targets for Chaos Monkey. This "opt-out" model encourages a culture where services are built to be resilient from the start. Teams must explicitly disable Chaos Monkey for a service if it is not ready to withstand random instance termination.

### 3. Scheduled Terminations
Chaos Monkey is configured to run only during normal business hours. This ensures that engineers are on hand and ready to respond if a termination uncovers a real problem. It is not meant to be a surprise attack but a planned, continuous test.

### 4. Random but Controlled Chaos
Within its schedule, Chaos Monkey randomly selects an instance from an eligible cluster and terminates it. The frequency and targeting of these terminations are configurable, allowing you to control the level of chaos injected into your system.

## Why Use Chaos Monkey?

* **Builds Resilient Services:** It forces engineers to design services that can handle unexpected failures gracefully, for example, by running multiple redundant instances behind a load balancer.
* **Identifies Hidden Weaknesses:** It uncovers problems that might not be apparent in normal testing, such as services that don't restart correctly, have improper fallback mechanisms, or are a single point of failure (SPOF).
* **Prevents Major Outages:** By finding and fixing these small issues proactively, you significantly reduce the risk that a simple instance failure could cascade into a major, customer-facing outage.
* **Drives a Culture of Reliability:** It shifts the engineering mindset from "assuming everything works" to "designing for failure."

## Key Components of Chaos Monkey

1.  **The Chaos Monkey Engine:** This is the core service that contains the logic for scheduling, selecting, and terminating instances. It runs as a standalone application.
2.  **Spinnaker Integration:** Spinnaker acts as the source of truth for your infrastructure. Chaos Monkey queries Spinnaker's API to get a list of applications and their instances.
3.  **Configuration:** The behavior of Chaos Monkey is controlled through a properties file (e.g., `chaos.properties`). This file defines the schedule, opt-out configurations, and integration details with Spinnaker and your cloud provider.

## Getting Started: A Conceptual Example

Deploying Chaos Monkey is an advanced task, but the conceptual flow is as follows:

1.  **Deploy Spinnaker:** Since Chaos Monkey relies on it, you must have a running Spinnaker instance that is managing your cloud resources.
2.  **Deploy Chaos Monkey:** Run the Chaos Monkey application, typically as a container or on a dedicated VM.
3.  **Configure Chaos Monkey:** Create a configuration file that points Chaos Monkey to your Spinnaker instance, defines its execution schedule, and sets up any initial exceptions.
4.  **Enable Chaos for an Application:** In your application's configuration within Spinnaker, ensure that Chaos Monkey is enabled (which it is by default).
5.  **Observe and Harden:** Monitor your application's health and alerts. When Chaos Monkey terminates an instance, does your service recover automatically? Does it affect users? If you find a weakness, fix it, and let Chaos Monkey continue to test your improvements.

## Further Reading & Community

* **Official GitHub Repository:** [https://github.com/Netflix/chaosmonkey](https://github.com/Netflix/chaosmonkey)
* **The Netflix Tech Blog (Origin Story):** [https://netflixtechblog.com/the-netflix-simian-army-16e57fbab116](https://netflixtechblog.com/the-netflix-simian-army-16e57fbab116)
* **Principles of Chaos Engineering:** [https://principlesofchaos.org/](https://principlesofchaos.org/)

---
We hope this guide has helped you understand the purpose and principles behind Chaos Monkey, the original tool of Chaos Engineering!