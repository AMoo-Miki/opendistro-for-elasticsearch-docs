---
layout: default
title: Trace Analytics
nav_order: 48
has_children: true
has_toc: false
---

# Trace Analytics

Trace Analytics provides a way to ingest and visualize [OpenTelemetry](https://opentelemetry.io/) data in Open Distro. This data can help you find and fix performance problems in distributed applications.

A single operation, such as a user clicking a button, can trigger an extended series of events. The front end might call a back end service, which calls another service, which queries a database, processes the data, and sends it to the original service, which sends a confirmation to the front end.

Trace Analytics can help you visualize this flow of events and identify performance problems.

![Detailed trace view](../images/ta-kibana-trace.png)
