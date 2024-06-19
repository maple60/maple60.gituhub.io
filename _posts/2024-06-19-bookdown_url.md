---
title: Bookdownで404エラーが出たときの原因の一つはURLエンコード
layout: post
tags: [R]
---

Rの[{bookdown}](https://bookdown.org)パッケージでドキュメントを作成したときに出会ったエラーについてのメモです。

## 状況

Synology NASで立ち上げたWebサーバー内にディレクトリを作成し、そこにbookdownのhtml outputのフォルダ(デフォルト名で`_book`)の中身をすべてコピーして外部からアクセスをできるようにしていました。

普通に閲覧ができていたのですが、あるページだけなぜか404エラーが出るので困っていました。

## 原因とその背景

結論としては、URLエンコードの問題でした。
そのファイルは`〇〇マップ.html`という名前のファイルで、日本語が入っていました。

ASCIIコードではない日本語は、URLに含めることができないので、ページ名が日本語の場合は、**URLエンコード**という処理で、URL内に表現できる形に置き換えられます。

例えば、日本語の「あ」であれば「%E3%81%82」に置き換えられます。
%と文字コードの組み合わせで変換をしているため、これを**パーセントエンコーディング**と呼ぶらしいです。

「マップ」という文字列は、通常「%E3%83%9E%E3%83%83%E3%83%97」という文字列で変換されます。
それぞれの対応としては、

- マ: %E3%83%9E
- ッ: %E3%83%83
- プ: %E3%83%97

という感じです。

ですが、bookdownでビルドされたファイルのリンクでは「%E3%83%9E%E3%83%83%E3%83%95%E3%82%9A」と変換されていました。
違いとしては、「プ」が「フ」と「ﾟ」(半濁点)が分けてエンコードされているところです。

つまり、本来は通常の「マップ」の方にリンクをしているはずが、リンクが半濁点のわけられた「マップ」にリンクしてしまっているために404エラー、つまり存在しないページに飛んでしまっていたということでした。

## 対策

とりあえず、レンダリング前のRmdファイルの見出しを「map」という感じで英語にしてこのエラーを回避しました。

bookdownははUTF-8でコーディングしているので、日本語や漢字も普通に利用できますが、URLにするとたまによろしくないことが起こってしまうようですね。

Rmdファイルのheader1(`#`)をそのままレンダリングしたhtmlファイルのファイル名にする仕様になっているので、今回のエラーを避けるとしたら、英語を使うのが無難かもしれません。

濁点と半濁点だけなのか、他の非ASCII文字でも起こるのかは未検証です。

## 最後に

見た目はどちらも「マップ」で同じため、今回のエラーは原因がわかるまでかなり時間がかかりました。

気づいた経緯は、アドレスバーにURLを手打ちしたらアクセスできたことからでした。
そこからアクセスできるときとできないときで、それぞれURLをエディタにコピペしたときにエンコーディングが異なっていることで解決できました。

日本人の方向けの書類は日本語で作成するのが良いですが、日本語は何かとエラーの原因ですね。
ドキュメントは基本的に英語で作成して、補足的に日本語を入れたほうが良いのかもと思った今回の件でした。

## 参考

URLエンコードについては、以下のサイト様を利用させていただきました。

[URLエンコード・デコード](https://tech-unlimited.com/urlencode.html)

ありがとうございました。