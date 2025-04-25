# Mock Exam 2
## Deploy a Horizontal Pod Autoscaler (HPA).
### Position of "metrics" in yaml.
[Ref](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/#:~:text=Open%20the%20/tmp/hpa%2Dv2.yaml%20file%20in%20an%20editor%2C%20and%20you%20should%20see%20YAML%20which%20looks%20like%20this%3A)
```
spec:
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
```

## Deploy a Vertical Pod Autoscaler (VPA).
### Solution.
```
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  name: multi-container-vpa
  namespace: default
spec:
  targetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: multi-container-deployment
  updatePolicy:
    updateMode: "Auto"

## Gateway, hostname, tlsの記述方法
```
# web-gateway.yaml
apiVersion: gateway.networking.k8s.io/v1
kind: Gateway
metadata:
  name: web-gateway
  namespace: cka5673
spec:
  gatewayClassName: kodekloud
  listeners:
    - name: https
      protocol: HTTPS
      port: 443
      hostname: kodekloud.com
      tls:
        certificateRefs:
          - name: kodekloud-tls
```
  resourcePolicy:
    containerPolicies:
    - containerName: backend-app
      mode: Auto
    - containerName: frontend-app
      mode: "Off"
```
