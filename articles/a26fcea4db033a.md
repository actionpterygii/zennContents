---
title: "Laravel11のlaravel.buildのsailで何もしてないのにエラー"
emoji: "🎂"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["laravel", "laravel11", "laravelsail"]
published: true
---
# 注意
できただけ
詳しくない
# 結論
`.env`ファイルの
`DB_DATABASE`を``に
`DB_DATABASE`を``に

とりあえずlocalhostでアクセスしてはじめましてページみたいなのが出るようになりました

# 動き
ターミナルで`curl -s https://laravel.build/プロジェクト名 | bash`を実行

パスワードを入力して完了

`cd プロジェクト名`

`sail up -d`

localhostにアクセス

```
Illuminate\Database\QueryException

SQLSTATE[HY000] [1044] Access denied for user 'sail'@'%' to database 'laravel'

SELECT * FROM `sessions` WHERE `id` = FbY68jCJQ4XxdRwnlJyCym1y4vSDlx5UvJoOvUVY limit 1
```
とか出る

ネットで拾った情報をもとに`.env`の
`DB_USERNAME=sail`
を
`DB_USERNAME=root`
に

`sail down`からの`sail up -d`

localhostにアクセス

```
Illuminate\Database\QueryException

SQLSTATE[HY000] [1049] Unknown database 'laravel'

SELECT * FROM `sessions` WHERE `id` = L4TSKYbVy4nxDdUcG2z61NOBd0H1saa0RlWyjrLe limit 1
```
右に
```
Database name seems incorrect
You're using the default database name laravel. This database does not exist.

Edit the .env file and use the correct database name in the DB_DATABASE key.
Database: Getting Started docs(https://laravel.com/docs/master/database#configuration)
```
と出る

困って`.env`の値をいろいろ変えて試してみる
直接mysqlのコンテナにアクセスしようとしても同じようなエラー内容で断られる

`DB_DATABASE=laravel`
を
`DB_DATABASE=mysql`
にする

localhostにアクセス

解決

# 謎
記事を書くために二回目で同じことを行ったときはならなかったが
一回目ではこれで解決せず、何らかのエラー画面がブラウザに出て
右のmigrationなんたらみたいな緑のボタンを押したらくくるまわってあくせすできるようになった
お助けやってあげるよありがとう機能的な？
デバッグモードを本番でonにしないようにしようとよりおもいました

# 環境
PHP 8.3.4
Laravel 11.0.7

# ちょっと前
ちょっと前（その時もlaravel11.0.7だったはず）には何もしなくてもlocalhostでアクセスできたはずなんですが今日やってできなくなった理由がよくわかりませんでした
環境も同じで特に何も変えていないはず
どういう違いのあれでなっているのか
v10以前もそのようなことはなかったはず
