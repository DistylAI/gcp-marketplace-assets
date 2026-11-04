# Configuration

Replace `NAME` and `NAMESPACE` with the values used at install.

## Tenant and user names

Stored in ConfigMap `NAME-page-config` as `config.js`:

```js
window.CM_CONFIG = {"tenantName":"demo-tenant","userName":"Demo User"};
```

1. Edit it:
   ```sh
   kubectl edit configmap NAME-page-config -n NAMESPACE
   ```
2. Restart the pods. The file is mounted with `subPath` and does not
   update in running pods.
   ```sh
   kubectl rollout restart deploy/NAME -n NAMESPACE
   ```

## Scaling

```sh
kubectl scale deploy/NAME -n NAMESPACE --replicas=N
```

## Resources

| Container | CPU request | Memory request | Memory limit |
|---|---|---|---|
| `web` | 50m | 32Mi | 64Mi |
| `ubbagent` | 10m | 32Mi | 64Mi |

## Components

| Resource | Name |
|---|---|
| Application | `NAME` |
| Deployment | `NAME` |
| Service | `NAME` (ClusterIP, port 80 -> 8080) |
| ConfigMap | `NAME-page-config` |
| ConfigMap | `NAME-ubbagent` |

- `web`: nginx serving the page on port 8080.
- `ubbagent`: sidecar required by Google Cloud Marketplace. It reads the
  Marketplace reporting secret (keys `reporting-key`, `consumer-id`).
  Do not delete that secret; the pod will not start without it.

## Security

- All containers run as non-root.
- Seccomp profile: `RuntimeDefault`.
- All Linux capabilities dropped; privilege escalation disabled.
- `ubbagent` runs with a read-only root filesystem.
