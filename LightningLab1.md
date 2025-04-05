# Print the names of all deployments in the admin2406 namespace in the following format...
- custom-columnsの使い方 [Docs](https://kubernetes.io/docs/reference/kubectl/#custom-columns)
```
kubectl -n admin2406 get deployment -o custom-columns=DEPLOYMENT:.metadata.name,CONTAINER_IMAGE:.spec.template.spec.containers[].image,READY_REPLICAS:.status.readyReplicas,NAMESPACE:.metadata.namespace --sort-by=.metadata.name > /opt/admin2406_data
```
