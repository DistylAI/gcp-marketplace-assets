# Uninstall

Replace `NAME` and `NAMESPACE` with the values used at install.

1. Delete the Application. Its Deployment, Service, and ConfigMaps are
   owned by it and are removed with it.
   ```sh
   kubectl delete application NAME -n NAMESPACE
   ```
2. Optional: delete the reporting secret.
   ```sh
   kubectl get secrets -n NAMESPACE
   kubectl delete secret SECRET_NAME -n NAMESPACE
   ```
3. Optional: delete the namespace if nothing else uses it.
   ```sh
   kubectl delete namespace NAMESPACE
   ```

The app can also be deleted from the GKE console under
**Applications**.
