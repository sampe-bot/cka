# How to write code for "nslookup" in Pod to see another Pod, Service about DNS.
```
k run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup <SVC PORT> 
```
- ```--rm``` : Pod終了後、自動削除
- ```-it``` : インタラクティブにコンテナ内でコマンドを実行可能
- ```--restart=Never``` : 一度実行した後は、再起動せずに終了
