---
title: "Concept - Kubernetes API Server Proxy"
type: concept
created: 2026-04-30
updated: 2026-04-30
sources:
  - "[[Source - Access Services Running on Clusters]]"
tags:
  - concept
  - kubernetes
  - networking
---

# Concept - Kubernetes API Server Proxy

The Kubernetes API server proxy is a mechanism that allows external clients to access services running inside a cluster by routing requests through the API server. This avoids the need to expose services via LoadBalancers, NodePorts, or Ingress controllers for internal debugging and ad-hoc access.

## Explanation

When you run `kubectl cluster-info`, Kubernetes returns proxy URLs for services in the cluster. Rather than relying solely on `kubectl proxy` or port-forwarding, you can manually construct URLs that traverse the API server to reach service endpoints directly. This is useful for:

- Debugging services without modifying their exposure type
- Accessing internal dashboards (e.g., Elasticsearch, Prometheus)
- Scripting automated health checks against cluster-internal endpoints

The API server validates and forwards the request, so it acts as an authenticated gateway.

## Key Properties

- **URL structure**: `http://<master>/api/v1/namespaces/<ns>/services/[https:]<svc>[:port]/proxy`
- **Protocol override**: Prefix service name with `https:` to proxy over HTTPS
- **Port flexibility**: Use named ports, port numbers, or omit for the default port
- **Authentication**: Requests go through the API server, so existing kubeconfig credentials apply

## Connections

- Related concepts: [[Concept - Kubernetes Service Networking]], [[Concept - kubectl Port Forwarding]]
- Related commands: `kubectl cluster-info`, `kubectl get --raw`

## Sources

- [[Source - Access Services Running on Clusters]]

## Related

- [[index]]
