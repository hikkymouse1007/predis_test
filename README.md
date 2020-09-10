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
3. composer update or　新規プロジェクト作成

```
composer update
# or
composer create-project --prefer-dist laravel/laravel .
```
4. .envにapp/.env.exampleをコピー or env.redisをコピー
```
cp app/.env.example app/.env
# or
cp .env.redis app/.env
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

6. keyを生成(なければ)

```
php artisan key:generate
```

7. migrate

```
php artisam migrate
```

### 設定するファイル内容
最低限設定されていれば接続できるファイル群を以下に書く。

```
# .env
~~~
BROADCAST_DRIVER=log
CACHE_DRIVER=redis
QUEUE_CONNECTION=sync
SESSION_DRIVER=redis
SESSION_LIFETIME=120

REDIS_HOST=redis
REDIS_PASSWORD=null
REDIS_PORT=6379
~~~~
```

```
~~~
# config/database.php
'redis' => [

        'cluster' => false,
        'client' => 'predis',

        'default' => [
            'host'     => 'redis',
            'port'     => 6379,
            'database' => 0,
        ],

        'cache' => [
            'host'     => 'redis',
            'port'     => 6379,
            'database' => 0,
        ],

    ],
~~~~
```

```
# .env
~~~
BROADCAST_DRIVER=log
CACHE_DRIVER=redis
QUEUE_CONNECTION=sync
SESSION_DRIVER=redis
SESSION_LIFETIME=120

REDIS_HOST=redis
REDIS_PASSWORD=null
REDIS_PORT=6379
~~~~
```

### メモ
- redisコンテナのパスワードはdocker-composeと同じディレクトリの.envから読み込む。(今回は不使用)
docker-composeへは同じ階層に.envを置くと環境変数を渡すことができる。

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
- phpredis(今回はpredisを使用)
 https://stackoverflow.com/questions/31369867/how-to-install-php-redis-extension-using-the-official-php-docker-image-approach
- 公式
https://readouble.com/laravel/8.x/ja/redis.html
- Laradock-redis
https://laradock.io/documentation/#use-redis

- Laravelのredis周りのファイル設定
https://qiita.com/minato-naka/items/8b31d28823cabaa9487a#laravel%E3%81%AEredis%E5%88%A9%E7%94%A8%E8%A8%AD%E5%AE%9A
