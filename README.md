# yii2
yii2.0 のキャッチアップ用の環境

## 構成

```
yii2/
├── README.md
├── backend/
│   ├── _api
│   └── _web
└── docker/
    ├── api
    ├── db
    ├── docker-compose.yml
    └── web
```

### 各環境について
* `backend`上にyii2の環境を二つ用意(中身は初期状態なので、用途は任意)
  * `_api/` ... restful API用を想定
    * http://localhost:10180/ 
    * 例) [RESTful ウェブ・サービス - yii](https://www.yiiframework.com/doc/guide/2.0/ja/rest-quick-start) の学習用
  * `_web/` ... モノリシック webアプリケーション用を想定
    * http://localhost:10181/ 
      * 例) [始めよう - yii](https://www.yiiframework.com/doc/guide/2.0/ja/start-workflow)の学習用
* phpmyadmin
  * http://localhost:8080/ 

## 手順

### 初回起動手順

```bash
# コンテナ起動時
$ cd docker 
$ docker-compose up -d 
```

- [localhost:10181](http://localhost:10181/)にアクセスすると、必要なパッケージをインストールしていないため、以下エラーが出る

```
Warning: require(/var/www/html/yii-app/web/../vendor/autoload.php): Failed to open stream: No such file or directory in /var/www/html/yii-app/web/index.php on line 9

Fatal error: Uncaught Error: Failed opening required '/var/www/html/yii-app/web/../vendor/autoload.php' (include_path='.:/usr/local/lib/php') in /var/www/html/yii-app/web/index.php:9 Stack trace: #0 {main} thrown in /var/www/html/yii-app/web/index.php on line 9
```

- よって、以下composerでyiiと必要なパッケージをインストールする 

```bash
# ローカルの yii2/docker の位置から webコンテナに入る
$ docker-compose exec web bash

# webコンテナに入り、/var/www/html/yii-app に移動
$ cd yii-app/
# プロジェクトのcomposer.jsonファイルがある位置で、yii、関連パッケージをインストール
$ composer install
# 完了後、コンテナを抜ける
$ exit
```

- [localhost:10181](http://localhost:10181/)にアクセスできるようになる

- `_api`も同様の流れで行う。

```bash
# ローカルの yii2/docker の位置から apiコンテナに入る
$ docker-compose exec api bash

# webコンテナに入り、/var/www/html/yii-app に移動
$ cd yii-app/
# プロジェクトのcomposer.jsonファイルがある位置で、yii、関連パッケージをインストール
$ composer install
# 完了後、コンテナを抜ける
$ exit
```

### 以降の操作手順

- 初回起動後以降は以下手順

```bash
# コンテナ起動時
$ cd docker 
$ docker-compose up -d 
# コンテナ停止時
$ docker-compose stop
# コンテナを消す
$ docker-compose down
```