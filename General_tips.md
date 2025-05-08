# How to write code for "nslookup" in Pod to connect another Pod, Service about DNS.
```
k run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup <SVC NAME> 
```
- ```--rm``` : Pod終了後、自動削除
- ```-it``` : インタラクティブにコンテナ内でコマンドを実行可能
- ```--restart=Never``` : 一度実行した後は、再起動せずに終了

# How to find jsonpath of node info
```
kubectl get nodes -o json | jq -c 'paths' <INFOMATION YOU'D LIKE TO KNOW>
# ex
kubectl get nodes -o json | jq -c 'paths' | grep type | grep -v condition
```
To know Internal IP Address
```
kubectl get nodes -o jsonpath='{items[*].status.addresses[0].address}'
```
# ファイル内文字列の全置換
sedは文字列を置換して出力するだけなので、リダイレクトで内容を上書きする必要がある
```
sed 's/旧文字列/新文字列/g' ファイル名 > ファイル名
```
