---
title: "Source - Access Services Running on Clusters"
type: source
created: 2026-04-30
updated: 2026-04-30
tags:
  - source
  - kubernetes
  - networking
---

# Source - Access Services Running on Clusters

Original file: `raw/Access Services Running on Clusters.md`

Source URL: <https://kubernetes.io/docs/tasks/access-application-cluster/access-cluster-services/#manually-constructing-apiserver-proxy-urls>

## Summary

This Kubernetes documentation page explains how to manually construct API server proxy URLs to access services running inside a cluster. The `kubectl cluster-info` command returns a base proxy URL, and users can append service endpoints, suffixes, and parameters to reach specific services without exposing them directly.

## Key Takeaways

- The API server can proxy requests to cluster services via a consistent URL pattern
- Proxy URL format: `http://<kubernetes_master_address>/api/v1/namespaces/<namespace_name>/services/[https:]<service_name>[:port_name]/proxy`
- If no port name is specified, the default or unnamed port is used; port numbers can also substitute for port names
- HTTPS proxying requires prefixing the service name with `https:` in the URL path
- Supported `<service_name>` formats:
  - `<service_name>` — HTTP to default/unnamed port
  - `<service_name>:<port_name>` — HTTP to specified port
  - `https:<service_name>:` — HTTPS to default/unnamed port (note trailing colon)
  - `https:<service_name>:<port_name>` — HTTPS to specified port

## Notable Claims / Quotes

> `http://`_`kubernetes_master_address`_`/api/v1/namespaces/`_`namespace_name`_`/services/`_`[https:]service_name[:port_name]`_`/proxy`

## Examples

- Elasticsearch endpoint `_search?q=user:kimchy`:

  ```
  http://192.0.2.1/api/v1/namespaces/kube-system/services/elasticsearch-logging/proxy/_search?q=user:kimchy
  ```

- Elasticsearch cluster health via HTTPS:
  ```
  https://192.0.2.1/api/v1/namespaces/kube-system/services/https:elasticsearch-logging:/proxy/_cluster/health?pretty=true
  ```

## Context

- Author: Kubernetes Documentation
- Date: 2026-04-30 (clipped)
- Publication: kubernetes.io

## Related

- [[Concept - Kubernetes API Server Proxy]]
- [[index]]
