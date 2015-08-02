# Jekyll + GitHub Pages で技術メモ用Blogを構築してみた

## はじめに

markdown形式で技術ログを作成したくて、
調べた結果Jekyll + Github Pagesが良さそうだったので導入してみた

### Jekyllとは

Jekyll（ジキル）は静的サイトの生成を行うための、RubyGemsで配布されているRuby製のツール

下記参照
[Jekyllとはなにか](https://app.codegrid.net/entry/jekyll-introduction)

### GitHub Pagesに公開するための下準備

まずはGitHub Pages用のリポジトリを作成する

1. GitHubにて[Create New Repo](https://github.com/new)へ

2. Repository name に`[username].github.io`と入力する。
   ※[username]は自分のIDを入力

3. Initialize this repository with a READMEのチェックははずす

4. Create Repository のボタンをクリック


### Jekyllの下準備

Jekyllをgemにてインストールする

1. ターミナルより下記を実行
`$ gem install jekyll`


### Jekyll Themeを選ぶ

ページデザインを自分を作成するのは大変なので、
一旦公開されているJekyll Themeの中から「[lanyon](https://github.com/poole/lanyon)」を使用してみる

Themeを選ぶ際の参考サイト  
[Jekyll Theme](http://jekyllthemes.org/)
[jekyll/jekyll Jekyll Themes](https://github.com/jekyll/jekyll/wiki/Themes)

### Lanyonを使用したGitHub Pagesの作成

1. ターミナルよりGetHub Pageを置きたいディレクトリへ移動し以下を実行する  
`$ git clone https://github.com/poole/lanyon.git  [username].github.com`

2. Github上のリポジトリと紐付ける  
`$ git remote add origin <作ったリポジトリのURL>`  

3. リポジトリの作成  
`$ git init`  

4. ファイルをリポジトリに追加  
`$ git add -A`

5. commit  
`$ git commit -m "init"`  

6. push  
`$ git push -u origin master`  

http://<ユーザー名>.github.io/にアクセスにアクセスして表示されればOK

### _config.ymlの設定

表示されたので、自分のサイトように設定ファイル(_config.yml)を下記内容に修正する  
```
# Use of `relative_permalinks` ensures post links from the index work properly.
permalink:           pretty
relative_permalinks: true

# Setup
title:               John Blog #自分用に修正
tagline:             'Personal Tips,Memo,LifeLog' #自分用に修正
description:         'This is John Blog' #自分用に修正
url:                 https://johnokaz.github.io/ #自分用に修正
baseurl:             'https://johnokaz.github.io/' #自分用に修正
paginate:            5

# About/contact
author:
  name:              Johnokaz #自分用に修正
  url:               https://twitter.com/johnokaz #自分用に修正
  email:             johnokaz@gmail.com #自分用に修正

# Custom vars
version:             1.0.0

# Add #自分用に追加
github:
  repo:  https://github.com/johnokaz/johnokaz.github.io.git  
```

### サイドバーにArchiveとTagコンテンツ

サイトバーにArchiveとTagのコンテンツを入れたかったので、  
プロジェクトホームディレクトリ配下に下記２ファイルを追加  

- archive.md  

```
---
layout: page
title: Archives
---

{% for post in site.posts %}
  * {{ post.date | date_to_string }} &mdash; [ {{ post.title }} ]({{ post.url }})
{% endfor %}  
```

- tags.html

```
---
layout: page
title: Tags
---
<!-- Page code borrowed by dbtek -->
{% capture site_tags %}{% for tag in site.tags %}{{ tag | first }}{% unless forloop.last %},{% endunless %}{% endfor %}{% endcapture %}
{% assign tag_words = site_tags | split:',' | sort %}

<div >
    <ul >
    {% for item in (0..site.tags.size) %}{% unless forloop.last %}
      {% capture this_word %}{{ tag_words[item] | strip_newlines }}{% endcapture %}
      <li>
          <a href="#{{ this_word | replace:' ','-' }}-ref" >
            {{ this_word }}<span class="badge pull-right">{{ site.tags[this_word].size }}</span>
         </a>
      </li>
   {% endunless %}{% endfor %}
   </ul>
</div>

<div >
  {% for item in (0..site.tags.size) %}{% unless forloop.last %}
    {% capture this_word %}{{ tag_words[item] | strip_newlines }}{% endcapture %}
    <div id="{{ this_word | replace:' ','-' }}-ref">
      <h2 >Posts tagged  with {{ this_word }}</h2>
      <ul >
        {% for post in site.tags[this_word] %}{% if post.title != null %}
          <li ><a href="{{ site.BASE_PATH }}{{post.url}}">{{post.title}}</a> <span >- {{ post.date | date: "%B %d, %Y" }}</span></li>
        {% endif %}{% endfor %}
      </ul>
    </div>
  {% endunless %}{% endfor %}
</div>  
```

### ビルドしてデプロイ
`$ jekyll build`  
`$ git add -A`  
`$ git commit -a -m "update"`  
`$ git push -u origin master`  

### デプロイ確認
https://[username].github.io/  
にアクセスしてちゃんと表示されているか確認
