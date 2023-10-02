---
title: LaravelでHTMLをreturnするときにできないこと
tags:
    - Laravel
private: false
updated_at: "2023-10-01T17:34:05+09:00"
id: 58134a275cfce6f0761a
organization_url_name: null
slide: false
ignorePublish: false
---

## 課題

掌田津耶乃著の[Laravel 入門第 2 版](https://www.shuwasystem.co.jp/book/9784798060996.html)を読んでいて、Controller で html を return するときに想定していたような挙動にならなかった点があったのでメモをする。

## 環境

WSL2 Ubuntu 20.04.6 LTS

Laravel Framework 8.83.27

## 2023/10/02 追記

コメントでこうすればいいよと教えていただきまして、追記します。

ありがとうございます！

```PHP
Route::get('hello/{msg}', function($msg){
    return '<html><body><h1>' . $msg . '</h1></body></html>';
});
```

こういうふうに書くと、ヒアドキュメント使わずでできるみたいです。

変数部分にかっこをつけてしまっていましたが、この部分は PHP の書き方部分になるので、かっこがいらないということでした。

## やりたいこと

view テンプレートではなくて、生の HTML を返したい。

```PHP
Route::get('hello/{msg}', function($msg){
    $html = <<<EOF
    <html>
    <body>
    <p>{$msg}</p>
    </body>
    </html>
    EOF;
    return $html;
});
```

これはうまく動いて、$msg が展開されて表示される。

## できなかったこと

ヒアドキュメントで書かないといけないのか？と思い、以下のように変更してみた。

```PHP
Route::get('hello/{msg}', function($msg){
    return '<html><body><h1>{$msg}</h1></body></html>';
});
```

これは、実行すると、「{$msg}」とそのまま表示されてしまう。

おそらく、return のあと、シングルクォーテーションで囲っている部分が文字列として判定されているのかなあと思った。

それで以下のように試してみた。

```PHP
Route::get('hello/{msg}', function($msg){
    return '<html><body><h1>'{$msg}'</h1></body></html>';
});

Route::get('hello/{msg}', function($msg){
    return '<html><body><h1>' + {$msg} + '</h1></body></html>';
});
```

これはどちらもエラーで動かなかった。

文字列連結みたいなことをイメージして試してみたがだめだった。

## まとめ

とりあえず HTML に変数を入れてそれを展開したい場合で、テンプレートを使わずに返したいときには、ヒアドキュメントで変数にいったんおさめてから、それを return する必要がありそう？

実際のプロダクトでコードを書くとなったらほぼ使わなそうなところではある。
