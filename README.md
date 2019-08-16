# AmazonLinux2のLaravel + mysql5.7 + nginx 環境をansibleで構築する！
1. node-exporter    
   * インストール
   * サービス起動
   * 自動起動設定
2. php
   * インストール
   * 各種モジュールインストール  
   php72,php-cli,php-gd,php-mysqlnd,php-mbstring,php-mcrypt,php-pdo,php-xml,php-fpm,php-json  
   * php.ini設定  
3. php-fpm
   * インストール
   * nginx連携設定
   * サービス起動
   * 自動起動設定
4. nginx  
   * インストール
   * nginx.conf配置
   * virtualserver.conf配置
   * サービス起動
   * 自動起動設定
5. mysql
   * mariadb削除
   * mysql5.7インストール
   * 不要ユーザー削除
   * パスワード変更
   * database作成
6. composer
   * インストール
   * パス通す
# 使い方
## brewを使ってansibleインストール
```
brew install ansible

ansible --version
ansible 2.8.3
```
## ansible hosts編集
### hosts.exampleファイルをhostsにコピー
./ansible/hosts.example
```
cp hosts.example hosts
```

### 接続したいホストのipアドレスをhostsファイルに記載する。
/ansible/hosts
```
[aws]
13.230.4.231 #接続対象サーバ

[aws:vars]
ansible_user=ec2-user #接続するユーザー awsはec2-userでログインする。
ansible_ssh_private_key_file=pemキーがおいてあるフォルダ　#例 /Users/yuki/Desktop/test.pem

```
## 変数ファイルを環境に応じて変更
/vars/variables.yml
```
virtual_src: laravel.conf.j2
virtual_dest: /etc/nginx/conf.d/laravel.conf
nginx_src: nginx.conf.j2
nginx_dest: /etc/nginx/nginx.conf
virtual_server_name: example.com
virtual_root_path: /var/www/laravel/public

```
## ansible実行
### そのまま実行
```
ansible-playbook -i hosts playbook.yml
```
### テスト実行　※実際には実行されない
```
ansible-playbook -i hosts playbook.yml -check
```
### syntax check
```
ansible-playbook -i hosts playbook.yml --syntax-check
```
