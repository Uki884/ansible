# ansibleでAmazonLinux2 + Laravel + mysql5.7 + nginx + SSL 環境を構築する！
1. node-exporter    
   * インストール
   * サービス起動
   * 自動起動設定
2. php
   * 各種モジュールインストール  
   php72,php-cli,php-gd,php-mysqlnd,php-mbstring,php-mcrypt,php-pdo,php-xml,php-fpm,php-json  
   * php.ini設定  
3. php-fpm
   * nginx連携設定
4. nginx  
   * nginx.conf配置
   * virtualserver.conf配置
5. mysql5.7
   * mariadb削除
   * mysql5.7インストール
   * 不要ユーザー削除
   * パスワード変更
   * database作成
6. composer
   * インストール
7. project clone
   * backlogからクローン
8. let's encryptでSSL化
   * 指定したドメインをSSL化
9. Laravel 
   * .env コピー＆設定変更
   * composer install
   * php artisan key:generate
   * php artisan migrate
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
### variables.ymlをコピー
```
cp variables.example.yml variables.yml
```
### variables.yml修正
/vars/variables.yml
```
---
# nginx
virtual_server_name: example.com #ドメイン名
virtual_root_path: "{{git_repo_dest}}/public" #プロジェクトのrootパス
# db
db_name: ansible_test #db名
db_user: root
db_password: root
#git
git_repo_url: "https://{{ user_name }}:{{ password }}@****.backlog.jp/git/{{ git_repo_name }}.git"
git_repo_name: Laravel #リポジトリの名前
git_repo_dest: /www/laravel #clone先
git_branch_name: develop #cloneするブランチ
#Laravel .env
MAIL_DRIVER: sendmail
MAIL_HOST: locahost
MAIL_PORT: 25
MAIL_USERNAME: null
MAIL_PASSWORD: null
MAIL_FROM_ADDRESS: example@example.com
MAIL_FROM_NAME: example@example.com
#SSL(let's encrypt)
ssl_email: example@example.com

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
