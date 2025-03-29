# How to write code for "nslookup" in Pod to connect another Pod, Service about DNS.
```
k run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup <SVC PORT> 
```
- ```--rm``` : Pod終了後、自動削除
- ```-it``` : インタラクティブにコンテナ内でコマンドを実行可能
- ```--restart=Never``` : 一度実行した後は、再起動せずに終了

# How to find jsonpath of node info
```
kubectl get nodes -o json | jq | -c 'paths' <INFOMATION YOU'D LIKE TO KNOW>
# ex
kubectl get nodes -o json | jq | -c 'paths' | grep type | grep -v condition
```
To know Internal IP Address
```
kubectl get nodes -o jsonpath='{items[*].status.addresses[0].address}'
```
