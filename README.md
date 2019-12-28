
# tokinowarini

### 関連ページ

- [tokinowarini のgitリポジトリ](https://github.com/i-makinori/tokinowarini)
- [tokinowarini の原稿試験](http://localhost:1313/)
- [i-makinori.github.io のgitリポジトリ](https://github.com/i-makinori/i-makinori.github.io/)
- [i-makinori.github.io の公開ページ](https://i-makinori.github.io/)


以下､hugoやら､github pagesやら､解らないなりにメモ

# ウェブログの追加や更新の方法

```
$ hugo new posts/<post_file_name>.md
```

により､記事ファイルの型枠が作成され､その場所に記事を [markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) にて書き込んでいく｡

```
$ hugo server -w
```

により､ファイルが更新する毎に､公開用のファイルも更新されるserverがlocalhostが生成され､公開時の様子を見ながら編集できるようになる｡ `-D` オプションにより､下書き(Draft)も見られるようになる｡

```
$ ./deploy.sh
```

により､記事自体のリポジトリ､公開用ファイルのリポジトリ毎に､更新がcommitとして生成され､pushされる｡

これにより､更新は公開される｡

## github pages 上にウェブログを配置するまで｡

### git, hugo をインストールする｡

Arch linuxでは､

```
$ sudo pacman -S git hugo
```


### github上にレポジトリを作成する｡

1. `サイトの名前`
  小生は`tokinowarini`とした｡ `content` 以下などには､皆様も大好きなmarkdown形式などにて､ウェブログの原稿を配置するなどしてゆく｡
  また､`config.toml`による定数設定や､`themes/`による型枠の定義やら引用など､各種の設定ファイルもこのリポジトリに配置され､原稿達がどのように変換されるのかも設定される｡
  `public` 以下は､<username>.github.io にリポジトリが紐づけされる｡
1. `<username>.github.io`
  原稿などはhtmlまで変換されたり複製されたりして､
  URLに対応してブラウザに解釈されるファイル｡
  静的ファイルはコピーされたり､原稿はhtml形式にまで｡
  サブドメインのルートディレクトリ`https://<username>.github.io/`は リポジトリ`<username>.github.io`のルートディレクトリ`https://github.com/<username>/<username>.github.io/`に対応する｡


### hugoにより､ローカル環境にblogのデータの構造のデータをディレクトリとして作成をする｡

```
$ hugo new site `サイトの名前`
```
により､実行したディレクトリ以下に `サイトの名前` が生成され､  
`サイトの名前` 以下に､記事の内容(content)､型枠(themes)､定数の設定(config.toml)､そして､ウェブログとして公開される実際のファイル群(public)､などが含有される｡


### hugoによる生成に向けて､型枠theme を選択する｡

自作などという真似はせずに､大人しく与えられたものを使う｡

[Hugo Themes](https://themes.gohugo.io/)から､好きな型枠を選択し､選択に対応して出てくる説明に従う(?)｡

[minimal-bootstrap-hugo-theme](https://themes.gohugo.io/minimal-bootstrap-hugo-theme/) の場合､ [homepage on github](https://github.com/zwbetz-gh/minimal-bootstrap-hugo-theme)
```
$ git submodule add https://github.com/zwbetz-gh/minimal-bootstrap-hugo-theme.git themes/minimal-bootstrap-hugo-theme
```

をし､ config.tomlの`theme`を､

```
theme = "minimal-bootstrap-hugo-theme"
```

とするなど､説明をもとに書き換える｡


### 定数など(?)を､をconfig.tomlにて設定する

適当に設定する｡

### ウェブログの記事を作成する｡

```
$ hugo new posts/sazare_naru_ishi.md
```

により､ `content/posts/` 以下に､記事毎の共通データが入力された記事のファイルが生成される｡ この場合は､`sazare_naru_ishi.md`である

```
---
title: "sazare_naru_ishi"
date: 2019-12-26T20:53:43+09:00
draft: true
---
```

`draft`を`false`とすることで､ソースコードの変換に適用されるようになり､公開されるようになる(?)｡

タグやカテゴライズとやらの設定もできるらしい｡

```
---
title: "さざれなるいし"
date: 2019-12-26T20:53:43+09:00
draft: false
categories: ["Ishi"]
tags: ["Sazare","Ishi","Rekiseki"]
---
```

残り本文は､markdownされてゆく｡

記事の確認は､

```
hugo server -D
```

にて｡


### 目的の差異を理由として､リポジトリを分割する｡

```
$ rm -rf public
$ git submodule add -b master https://github.com/<username>/<username>.github.io public
```

### 安定化させるべく､または､公開するべく､変更をリモートリポジトリに押し込む｡

記事が思い通りであることを確認した後に､

記事を公開する場合にはコマンドを実行してゆくが､面倒なので､スクリプトを利用する｡

`ウェブログ名`がhugoにpage initされているgitのリポジトリで､themeがhugo-bootstrap-premiumが設定され､publicがgithub pagesのindexを対応に持つgithub.ioリポジトリのsubmoduleで､あるなどの時に､

記事などを実際に公開されるファイル群にまで再生成し､日付を更新のメモとして､ふたつのリポジトリに更新を押し込む｡

```
$ ./deploy.sh
```

`deploy.sh` の内容は､[HUGOでブログ作成 → GitHub Pagesで公開する手順 ](https://chanmitsu55.github.io/2017/12/25/2017-12-25-create-blog-by-hugo/) の､ほぼ引用です｡



## 参考

- [HUGOでブログ作成 → GitHub Pagesで公開する手順 ](https://chanmitsu55.github.io/2017/12/25/2017-12-25-create-blog-by-hugo/)
  実際のウェブログの作成において参照したのはほぼこれです｡この纏めの作成にあたっては､ほぼパクリです｡
  技術的に気になりそうな処への資料へのリンクなども逐次配置されていて､再現性は高くなりそうです｡
- [Host on GitHub](https://gohugo.io/hosting-and-deployment/hosting-on-github/)
  Host on Github
- [Minimal Bootstrap Hugo Theme](https://themes.gohugo.io/minimal-bootstrap-hugo-theme/)
  小生がさらりと見て､最も落ち着いた型枠themeです｡
- [【2018年版】Hugoとgithub pagesでブログ作る方法【Circle CIも回します】](https://qiita.com/ryoma-tokushige/items/eba3e6cd415e9755af87)
  手法を明確に纏めているような気がします｡
  同一リポジトリに意味が異なってしまうbranchを切っているように見えたり､Circle Clを試したことが無いなどの理由で､この手法はやめておきました｡ 好みの問題と､小生の不勉強です｡


## その他

ウェブログの記事に含まれると意図する､画像や音楽などの静的ファイルは､､､content以下に置けばよいのか? staticに置くのは､､､嫌だなぁ､､､｡


# github pages と hugo によるブログの配置に関して思うところ
### 静的Webファイル生成プログラムとgithub pagesによる
