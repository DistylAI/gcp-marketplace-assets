# Installation

## Prerequisites

- A GKE cluster with `linux/amd64` nodes.
- `kubectl` configured for the cluster.
- Free capacity per replica: about 100m CPU and 100Mi memory.

## Install from Marketplace

1. Open the Distillery listing in Google Cloud Marketplace and click
   **Configure**.
2. Select the cluster and set the parameters:

   | Parameter | Default | Notes |
   |---|---|---|
   | App instance name | none | Used as the name of all resources. |
   | Namespace | none | Target namespace. |
   | Replicas | `1` | Number of web pods. Minimum 1. |
   | Tenant name | `demo-tenant` | Shown in the page header. |
   | User name | `Demo User` | Shown in the sidebar. Initials fill the avatar. |

3. Click **Deploy**. Marketplace creates the reporting secret.

## Verify

```sh
kubectl get application,deploy,svc,pods -n NAMESPACE
```

- The `Application` reports Ready.
- Each pod runs two containers: `web` and `ubbagent`.

## Open the page

The Service is `ClusterIP` on port 80. Forward it locally:

```sh
kubectl port-forward -n NAMESPACE svc/NAME 8080:80
```

Then browse to http://localhost:8080.

For external access, add an Ingress or a `LoadBalancer` Service that
targets `svc/NAME` port 80.
