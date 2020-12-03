# Rails共同開発講座

## Member

- 名前: Tatty
  - 好きなメソッド: map
- 名前: ryoji
  - 好きなメソッド: present


## Docker操作
ゼロからdocker環境を立ち上げる場合は、上から順にコマンドを実行すればOK
### docker imageのビルド
```
docker-compose build
```
### docker-compose起動
```
docker-compose up
```
※clone後の初回起動時（`docker-compose up`実行後）に「Dockerコンテナが正常に起動しない問題」が発生するので、
`Ctrl + C`でコンテナを停止し、「Dockerコンテナが正常に起動しない問題の対処法」に従い実行する。


※`docker-compose up`で正常に起動すると下記のようなログが出力される。
```
web_1  | （省略）
web_1  | * Environment: development
web_1  | * Listening on tcp://0.0.0.0:3000
web_1  | Use Ctrl-C to stop
```

### docker-composeで起動しているコンテナを確認する方法
```
docker-compose ps
```
※起動している場合：State = Up 、停止している場合：State = Exit

### docker環境でbundle installを実行する
###### docker-composeを起動中の場合
```
docker-compose exec web bundle install
```
###### docker-composeを起動していない場合
```
docker-compose run web bundle install
```
### railsコマンドを実行する
##### docker-composeで起動しているコンテナに入る
コンテナに入った後、railsコマンドが実行できる
```
docker-compose exec web bash
```
※コンテナに入ると以下の様な表示になり、ターミナルと同じようにコマンド入力できる。
```
root@c04fbf9df6e7:/myapp#
```

- コンテナ内で、DBを作成する
```
# rails db:create
```
- コンテナ内で、マイグレーション実行
```
# rails db:migrate
```
※railsページが見れるようになります。http://localhost:3000/

- コンテナから出る
```
# exit
```

### docker-composeコンテナを停止する方法
```
docker-compose stop
```

### ログの確認方法
```
docker-compose logs
```
### 停止中のdocker-composeコンテナを削除する方法
※対象：カレントディレクトリのdocker-composeコンテナ
```
docker-compose rm
```
### Dockerのイメージ、コンテナ、ネットワーク、ボリューム一括削除
- 未使用なイメージ、コンテナ、ネットワークを一括削除（volumeも含め全て削除）
```
docker system prune -a --volumes
```
### Dockerコンテナ一覧表示
```
docker ps

docker-compose ps
```
※-aオプションをつけると終了したコンテナも表示される


## [clone後 初回起動] Dockerコンテナが正常に起動しない問題の対処法

#### 発生状態
`docker-compose up`を実行後にdockerコンテナが正常に起動せず、logに以下の様に出力されます。
```
========================================
  Your Yarn packages are out of date!
  Please run `yarn install --check-files` to update.
========================================
```

#### 対処法

以下の方法で、yarnをインストールしてください。（5分ほどかかります。）
```
$ docker-compose run --rm web yarn install
```

実行が完了すると下記のようにログが出力されます
```
[4/4] Building fresh packages...
Done in 366.80s.
```

最後に以下コマンドにより正常に起動します。
```
$ docker-compose up
```
[[この問題についての詳細はこちら]](https://qiita.com/yama_ryoji/items/1de1f2e9e206382c4aa5)




