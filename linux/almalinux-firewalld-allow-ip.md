# AlmaLinuxのfirewalldで特定IPだけ許可する設定方法
firewalldは特定のサービスに縛られず、ネットワークレベル（TCP,UDPやポート番号）でコントロール可能な為汎用的。

<b>※※設定をミスすると、サーバーから閉め出される可能性あるため、必ずサーバー接続されたターミナルを生かして作業をする。</b>

## firewalld設定手順
### ①firewallが起動中であるか確認する。
```
- sudo firewall-cmd --state
  - 起動中 = running
```

### ②ランタイムに追加（一度の永続化より安全）
```
sudo firewall-cmd --zone=public \
  --add-rich-rule='rule family="ipv4" source address="xxx.xxx.xxx.xxx" port port="22" protocol="tcp" accept'
```
### ③ランタイムのまま別ターミナルで対象IPからSSH接続テスト、問題なければ永続化
```
sudo firewall-cmd --permanent --zone=public \
  --add-rich-rule='rule family="ipv4" source address="xxx.xxx.xxx.xxx" port port="22" protocol="tcp" accept'
```

### ④firewalldの設定を再読み込み
```
sudo firewall-cmd --reload
```
### ⑤IPが許可リストに追加された確認

```
sudo firewall-cmd --zone=public --list-all
```