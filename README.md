### チュートリアル
https://aregsar.com/blog/2020/laravel-app-with-redis-in-docker/

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


