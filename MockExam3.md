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
