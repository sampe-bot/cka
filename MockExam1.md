# Multi containers, shared_volume sidecar and logging
```
apiVersion: v1
kind: Pod
metadata:
  name: mc-pod
  namespace: mc-namespace
spec:
  containers:
    - name: mc-pod-1
      image: nginx:1-alpine
      env:
        - name: NODE_NAME
          valueFrom:
            fieldRef:
              fieldPath: spec.nodeName
    - name: mc-pod-2
      image: busybox:1
      volumeMounts:
        - name: shared-volume
          mountPath: /var/log/shared
      command:
        - "sh"
        - "-c"
        - "while true; do date >> /var/log/shared/date.log; sleep 1; done"
    - name: mc-pod-3
      image: busybox:1
      command:
        - "sh"
        - "-c"
        - "tail -f /var/log/shared/date.log"
      volumeMounts:
        - name: shared-volume
          mountPath: /var/log/shared
  volumes:
    - name: shared-volume
      emptyDir: {}
```
# Helm
List chart repositories
```
helm repo ls
```
Lists all of the releases for a specified namespace
```
helm repo ls -A # for all namespace
helm repo ls -n NAMESPACE # for a specific namespace
```
Update gets the latest information about charts from the respective chart repositories.
```
helm repo update
```
Upgrades a release to a new version of a chart
```
# RELEASE, CHARTはrepo ls -Aで確認できる
helm upgrade <RELEASE> <CHART> <NAMESPACE> --version=<VERSION>
# ex
helm upgrade kk-mock1 kk-mock1/nginx -n kk-ns --version=18.1.15
```
