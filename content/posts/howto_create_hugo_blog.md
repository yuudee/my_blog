+++
title = 'HugoとGithub pagesでブログサイトを構築する'
date = 2024-07-01T14:03:33+09:00
draft = false
isStarred =  false
toc = true
tocOpen = true
+++

## はじめに
今回は静的サイトジェネレータ`Hugo`を用いてブログサイトを構築・Github pagesにデプロイする手順を紹介していきたいと思います．

以前までは全てのアウトプットをQiitaに投稿していましたが，Qiitaに投稿するまでもないようなアウトプットなどを残せる場所を残したいと思い，このサイトを作りました．

## Hugoのダウンロード
まずはHugoのインストールを行います．

この[サイト](https://github.com/gohugoio/hugo/releases)よりzipファイルをダウンロードします．


以下のサイトの
![hugoのインストール](/images/20240701/initialized_hugo.png)
今回はwindowsで構築するので`hugo_0.127.0_windows-amd64.zip`をダウンロードし任意の場所に回答します(＊以後は`hugo_path`とします)．

## 環境変数の設定
hugoのコマンドが実行できるようにpathを通します．
pathを通す場所は，先ほどのzipファイルの解凍先です．

## サイトの初期化
ここから新しいサイトの構築をしていきます．

コマンドプロンプトを開いて`hugo_path`に移動します．(pathを通した後に再起動などをすれば移動しなくてもいいかもしれません．)

`hugo new site {任意の名前}`

実行後，このようにいくつかのファイルが生成されます．

![initialized_hugo](/images/20240701/initialized_hugo.png)

## テーマの設定
Hugoには多くのテンプレートテーマが準備されています．

今回は[Hugo Themes](https://themes.gohugo.io/)の中の`beautifulhugo`というテーマを使用します．

gitよりテーマのファイルをクローンしてきます．(themeディレクトリで)

`git clone https://github.com/halogenica/beautifulhugo.git`

`hugo.toml`に以下の内容を追記します．

`theme = 'beautifulhugo'`

これで起動できます．

`hugo serve`

## 細かい設定について
細かい設定などは`beautifulhugo`の中の`example site`の中に書いてあるのでそちらをディレクトリごとコピーすればいいかなと思います．

### 記事の作成
以下のコマンドを実行して新しいファイルを作成します．
`hugo new post/{日付}/{ファイル名}.md`
私は画像を記事ごとのディレクトリに分けて保存しているため`{日付}`というディレクトリを間に挟んでいます．

### 記事のヘッダーについて
各記事のヘッダーは以下のように記述します．
```
---
title: {タイトル}
subtitle: {サブタイトル}
date: {投稿日}
tags: {タグ，複数指定する場合は，[]で囲む}
bigimg: [{src: {パス}, desc: {キャプション？}}]
---
```
また先ほどの`hugo new post/{日付}/{ファイル名}.md`などで新規ファイルを作成するとヘッダーのデフォルトが
```
+++
title:
subtitle:
+++
```
みたいな感じになりますが，そちらは`archetype/default.md`の中身を書き換えれば反映させることができます．

### Github pagesの利用
ここからは実際にブログとして運用するためにGithub pagesへのデプロイ方法について書いていきます．

まず適当はリポジトリを作成します．今回は`yuudee_blog`とします．

次にローカルのブログ用ディレクトリとリモートリポジトリを接続します．

`git remote add origin {リポジトリのURL}`

何か適当な記事を作成します．

コマンドラインで`hugo`と入力します．これによって記事用に作成したmarkdownファイルがHTMLに変換されwebページとして表示できるようになります．

{サイト名}以下のディレクトリを全てコミットします．この時`hugo`コマンドによって生成されたディレクトリを`docs`にしないとGithub pagesでデプロイできない？ようなので`hugo.toml`に以下の内容を追記します．

```toml:hugo.toml
publishDir = "docs"
```
Github側にコミットしたら`setting`に進み`pages`の項目を以下のように変更します．

![githubpages_setting](/images/20240701/githubpages_setting.png)

画像を埋め込みたい際には`static`ディレクトリに`images`というフォルダを作成し，その中に画像を入れていきます．

参照する際は`https://{ユーザ名}.github.io/{リポジトリ名}/images/{画像名}`のような形式で書くようにしてください

## (2024/09/14追記)
現在はこのテーマから[このサイト](https://github.com/hugo-sid/hugo-blog-awesome)に移行しました．