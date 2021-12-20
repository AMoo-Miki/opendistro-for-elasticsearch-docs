---
layout: default
title: Configuration
nav_order: 1
parent: Security
has_children: true
has_toc: false
---

# Security configuration

The plugin includes demo certificates so that you can get up and running quickly, but before using Open Distro in a production environment, you must configure it manually:

1. [Replace the demo certificates](../../install/docker-security/)
1. [Reconfigure elasticsearch.yml to use your certificates](tls/)
1. [Reconfigure config.yml to use your authentication backend](configuration/) (if you don't plan to use the internal user database)
1. [Modify the configuration YAML files](yaml/)
1. [Apply changes using securityadmin.sh](security-admin/)
1. Start Elasticsearch.
1. [Add users, roles, role mappings, and tenants](../access-control/)

If you don't want to use the plugin, see [Disable security](disable/).
