# matsupy-container

dockerコンテナのサンプル集です。
開発環境等ですぐに使えるものを取り揃えています。

## redmine-postgres/

redmineを構築するためのdocker compose資材。

## mysql/

mysql8.0とphpmyadminセットのdocker compose資材。

## nextcloud/

nextcloudのdocker compose資材。
mysqlとNASのSMB接続可能なnextcloud資材の構成。
.env で接続情報を管理し、共有時は .env.example を雛形として利用します。

### Ubuntuでの構築手順

Ubuntu ユーザー向けに、Nextcloud をローカルで起動するまでの流れをまとめます。

#### 1. git clone で資材を取得

任意の作業ディレクトリで、まずリポジトリを取得します。

```bash
git clone https://github.com/ryopan727/matsupy-container.git
cd matsupy-container/nextcloud
```

#### 2. .env を確認または作成

この構成では、データベース接続情報やポート番号を .env で管理します。

.env がまだ無い場合は、雛形から作成します。

```bash
cp .env.example .env
```

.env を編集して、最低限次の項目を確認してください。

```dotenv
NEXTCLOUD_DB_HOST=mysql
NEXTCLOUD_DB_NAME=nextcloud
NEXTCLOUD_DB_USER=nextcloud
NEXTCLOUD_DB_PASSWORD=change-me
MYSQL_ROOT_PASSWORD=change-me
NEXTCLOUD_PORT=8000
```

初学者のうちは、NEXTCLOUD_DB_HOST は mysql のまま変更しないでください。パスワードだけ自分用の値に変えれば十分です。

#### 3. コンテナを起動

次のコマンドでビルドと起動を行います。

```bash
docker compose up -d --build
```

起動中のコンテナを確認します。

```bash
docker compose ps
```

#### 4. ブラウザで初期設定

ブラウザで次の URL を開きます。

```text
http://localhost:8000
```

初回表示では Nextcloud の管理者アカウント作成画面が表示されます。

- 管理者アカウント名: 好きな名前を入力
- 管理者パスワード: 強いパスワードを入力
- データベースユーザ: .env の NEXTCLOUD_DB_USER
- データベースパスワード: .env の NEXTCLOUD_DB_PASSWORD
- データベース名: .env の NEXTCLOUD_DB_NAME
- データベースホスト: mysql

入力後にインストールを実行すれば、Nextcloud の初期構築が完了します。

#### 5. 停止と再起動

停止:

```bash
docker compose down
```

再起動:

```bash
docker compose up -d
```

データは Docker ボリュームに保存されるため、通常の docker compose down では消えません。

#### 6. 困ったときの確認

ログ確認:

```bash
docker compose logs -f
```

Nextcloud コンテナだけ見る場合:

```bash
docker compose logs -f nextcloud
```

MySQL コンテナだけ見る場合:

```bash
docker compose logs -f mysql
```

ポート 8000 が既に使われている場合は、.env の NEXTCLOUD_PORT を 8080 などに変更してから再度起動してください。