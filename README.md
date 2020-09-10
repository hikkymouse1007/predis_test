### チュートリアル
https://aregsar.com/blog/2020/laravel-app-with-redis-in-docker/
本記事では、redisパスワードを設定しているが、
本リポジトリでは設定しない。
また、redisのパスワードを設定しない。

### セットアップ
1. ホームディレクトリに移動してdocker-composeを起動

```
docker-compose up -d
```
2. webコンテナに入る

```
docker-compose exec web bash
```
3. composer update

```
composer update
```
4. .envに.env.exampleをコピー
```
cp .env.example .env
```

5. .envのdb接続設定に注意

```
DB_CONNECTION=mysql
DB_HOST=db-host //コンテナ名
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=test
```

6. keyを生成

```
php artisan key:generate
```

7. migrate

```
php artisam migrate
```

### メモ
- redisコンテナのパスワードはdocker-composeと同じディレクトリの.envから読み込む。

```
# Also note the REDIS_PASSWORD setting in the redis startup command. This setting   will be defined in the applications .env file. Since the docker-compose.yml file is in the same directory as the .env file, it will make use of this setting.

/.env
REDIS_PASSWORD=myapp
```

- redisコンテナログイン & redis-cliの起動

```
docker exec -it myapp-redis sh

/date redis-cli
```

### 参考
phpredis: https://stackoverflow.com/questions/31369867/how-to-install-php-redis-extension-using-the-official-php-docker-image-approach