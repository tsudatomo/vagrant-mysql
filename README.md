# mysql replication sample vagrantfile

## description
2台のvagrantを mysql replication された状態で起動するようのVagrantfileとansible-playbook

## how to use
git clone
playbook(mysql.yml)のgithub account を自分のアカウントに修正
vagrant up

なぜgithub accountが必要か
mysql の replicationを設定する際にmaster db側のbin log pos を取ってくる際のssh用
なので、github accountに設定済みの公開鍵の対の秘密鍵ををデフォルトで利用するようになってないと動かないかも。

### required
- ansible 2.1.2.0
- vagrant 1.8.6

## tips
### server-id
server-idだけip から計算して設定するようになっているため、
vagrant起動環境によっては動作がおかしいかも。
`ansible_eth1.ipv4.address` あたりを変えれば動く気がします。

### sample data
https://dev.mysql.com/doc/index-other.html
から適当なのをDLするのが良い
world databaseとか、employee dataとか。

