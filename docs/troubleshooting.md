# Troubleshooting

Replace `NAME` and `NAMESPACE` with the values used at install.

| Symptom | Likely cause | Fix |
|---|---|---|
| Pod `CreateContainerConfigError` | Reporting secret missing or renamed. | Reinstall from Marketplace to recreate the secret. |
| `ubbagent` restarting or logging errors | Reporting secret invalid or revoked. | Check `kubectl logs deploy/NAME -c ubbagent -n NAMESPACE`. Reinstall if the secret is bad. |
| Pods `Pending` | Not enough CPU/memory, or no `linux/amd64` nodes. | `kubectl describe pod -n NAMESPACE`; add capacity or amd64 nodes. |
| Tenant/user name change not shown | Pods still use the old `config.js`. | `kubectl rollout restart deploy/NAME -n NAMESPACE` |
| Application not Ready | A component is failing. | `kubectl describe application NAME -n NAMESPACE` and check pod status. |
| Page unreachable | Service is ClusterIP only. | Use `kubectl port-forward` or add an Ingress/LoadBalancer. See [Installation](installation.md#open-the-page). |

## Contacting support

Include:

- Namespace and app instance name.
- Output of:
  ```sh
  kubectl get all,application,configmap -n NAMESPACE
  kubectl describe application NAME -n NAMESPACE
  kubectl describe pods -n NAMESPACE
  kubectl logs deploy/NAME -c web -n NAMESPACE
  kubectl logs deploy/NAME -c ubbagent -n NAMESPACE
  ```

Support: https://distyl.ai
