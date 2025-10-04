# FreePBX Asterisk - オープンソースPBX構築環境

FreePBX Asterisk は、オープンソースの PBX（Private Branch Exchange）システム「Asterisk」を Docker 環境で簡単に構築・実行できるリポジトリです。  
このプロジェクトでは、音声通話システムの構築・管理を容易にし、開発・検証環境を素早く立ち上げることができます。

---

## 📘 概要

FreePBX は、Asterisk をベースにした Web 管理インターフェースを提供する PBX システムです。  
このリポジトリでは、Docker Compose を利用して FreePBX 環境を簡単に構築できます。

---

## 🧩 主な機能

- Docker Compose による簡単なセットアップ  
- FreePBX Web 管理画面の提供  
- Apache / Asterisk の自動構成  
- ログ・パーミッション設定の自動化サポート  
- ローカル環境での動作確認が容易  

---

## 🏗️ セットアップ手順

### 1. ディレクトリ移動
```bash
cd app
```

### 2. Docker ネットワーク作成
```bash
docker network create nginx-proxy
```

### 3. コンテナ起動
```bash
docker-compose up -d
```

### 4. コンテナのステータス確認
```bash
docker-compose ps
```

### 5. FreePBX 管理画面へアクセス
ブラウザで以下の URL にアクセスします：
```
http://localhost/admin/config.php
```

---

## 🧰 トラブルシューティング

### Apache 設定エラー（ServerName 未設定）
初期設定では `ServerName` が未定義のため、以下の手順で修正します。

```bash
docker exec -it freepbx-app bash
vi /etc/apache2/apache2.conf
```

ファイル末尾に以下を追記：
```
ServerName localhost
```

設定確認：
```bash
apachectl configtest
```

`Syntax OK` が表示されたら Apache を再起動：
```bash
service apache2 restart
```

---

## 🪵 ログディレクトリ設定

### ログディレクトリ作成
```bash
mkdir -p /var/log/apache2
chown -R www-data:www-data /var/log/apache2
chmod -R 755 /var/log/apache2
```

再度設定確認：
```bash
apachectl configtest
```

---

## 🔐 パーミッション設定

ログの権限エラーが発生する場合は、以下の手順で修正します。

![ログ権限エラー](./imgaes/log_permission.png)

### コンテナに入る
```bash
docker exec -it freepbx-app bash
```

### 権限修正
```bash
chown -R asterisk:asterisk /var/log/asterisk
chmod -R 775 /var/log/asterisk
```

### ログファイル作成（必要に応じて）
```bash
touch /var/log/asterisk/freepbx.log
chown asterisk:asterisk /var/log/asterisk/freepbx.log
chmod 664 /var/log/asterisk/freepbx.log
```

### サービス再起動
```bash
service apache2 restart
service asterisk restart
```

---

## 🖥️ ログイン画面

![ログイン画面](./imgaes/login.png)

---

## 🧩 よくある操作

| 操作内容 | コマンド |
|-----------|-----------|
| コンテナ停止 | `docker-compose down` |
| コンテナ再起動 | `docker-compose up -d` |
| コンテナに入る | `docker exec -it freepbx-app bash` |
| Apache 設定確認 | `apachectl configtest` |

---

## 📚 参考資料

- [FreePBX Docker セットアップガイド](https://cloudinfrastructureservices.co.uk/how-to-setup-freepbx-using-docker-build-freepbx-docker-container/)
- [GitHub README 書き方ガイド](https://docs.github.com/ja/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
- [Qiita: READMEの書き方](https://qiita.com/shun198/items/c983c713452c041ef787)
- [C++ Learning: READMEの作り方](https://cpp-learning.com/readme/)
- [Reddit: GitHub README Templates](https://www.reddit.com/r/programming/comments/l0mgcy/github_readme_templates_creating_a_good_readme_is/?tl=ja)

---

## 🧑‍💻 ライセンス

このプロジェクトはオープンソースであり、自由に利用・改変できます。  
詳細は `LICENSE` ファイルをご確認ください。

---

## 💬 貢献方法

1. Issue を作成して改善提案を行う  
2. Fork してブランチを作成  
3. 修正後、Pull Request を送信  

---

## 🏁 作者

開発者: [Your Name]  
連絡先: [your.email@example.com]
