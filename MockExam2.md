# Mock Exam 2
## Deploy a Vertical Pod Autoscaler (VPA).
### Solution
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
  resourcePolicy:
    containerPolicies:
    - containerName: backend-app
      mode: Auto
    - containerName: frontend-app
      mode: "Off"
```
