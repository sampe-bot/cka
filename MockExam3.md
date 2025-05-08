# 通信確認用Podをワンライナーコマンドで作成する方法
```
k run curl --image=alpine/curl --rm -it -- sh
```

# Question1
Create a new service account with the name pvviewer. Grant this Service account access to list all PersistentVolumes in the cluster by creating an appropriate cluster role called pvviewer-role and ClusterRoleBinding called pvviewer-role-binding.
Next, create a pod called pvviewer with the image: redis and serviceAccount: pvviewer in the default namespace.
## Solution
Create serviceaccount called pvviewer
```
kubectl create serviceaccount pvviewer
```
Create clusterrole
```
kubectl create clusterrole pvviewer-role  --resource=persistentvolumes --verb=list
```
Create clusterrolebinding, サービスアカウントを有効化するため、```--serviceaccount=<Namespace>:<Serviceaccount-name>```を追加
```
kubectl create clusterrolebinding pvviewer-role-binding --clusterrole=pvviewer-role --serviceaccount=default:pvviewer
```
Manifest pod
```
---
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: pvviewer
  name: pvviewer
spec:
  containers:
  - image: redis
    name: pvviewer
  # Add service account name
  serviceAccountName: pvviewer
```
# NetworkPolicy
We have deployed a new pod called np-test-1 and a service called np-test-service. Incoming connections to this service are not working. Troubleshoot and fix it.
Create NetworkPolicy, by the name ingress-to-nptest that allows incoming connections to the service over port 80.
```
---
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: ingress-to-nptest
  namespace: default
spec:
  podSelector: # np-test-1のラベルを持ったpodのみ対象
    matchLabels:
      run: np-test-1
  policyTypes:
  - Ingress 
  ingress:
  - ports:
    - protocol: TCP
      port: 80
```
# Gateway API
Configure the web-route to split traffic between web-service and web-service-v2.The configuration should ensure that 80% of the traffic is routed to web-service and 20% is routed to web-service-v2.
```
kubectl create -n default -f - <<EOF
apiVersion: gateway.networking.k8s.io/v1
kind: HTTPRoute
metadata:
  name: web-route
  namespace: default
spec:
  parentRefs:
    - name: web-gateway
      namespace: default
  rules:
    - matches:
        - path:
            type: PathPrefix
            value: /
      backendRefs:
        - name: web-service
          port: 80
          weight: 80
        - name: web-service-v2
          port: 80
          weight: 20
EOF
```
# Adjust the following network parameters on the system to the following values, and make sure your changes persist reboots
- 設定出来たかどうかの確認
```
sysctl net.ipv4.ip_forward # ex1
sysctl net.bridge.bridge-nf-call-iptables # ex1
```
- 設定ファイル
```/etc/sysctl.d/k8s.conf```を編集する
sudo nano /etc/sysctl.d/k8s.confで開く or catを使用する
```
# catを使用する場合
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.ipv4.ip_forward = 1
EOF
```
再起動せずに設定ファイルを反映させる
```
sudo sysctl --system
```

