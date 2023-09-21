---
title: PHPフレームワークLaravel入門第2版を読むときに気をつけること
tags:
  - PHP
  - Laravel
private: false
updated_at: '2023-09-22T06:55:42+09:00'
id: 4e874766cd8273dffdd0
organization_url_name: null
slide: false
ignorePublish: false
---

## 課題

掌田津耶乃著の[Laravel 入門第 2 版](https://www.shuwasystem.co.jp/book/9784798060996.html)は Laravel 6 をベースにかかれているため、2023 年 9 月現在の最新バージョンでは動かないコードがある。

## 環境

WSL2 Ubuntu 20.04.6 LTS
Laravel Framework 8.83.27

## 本の通りでは動かないシリーズ

## p.43 のルート情報の書き方

### 問題

本の通りに書いても、クラスが見つからないというエラーが出る。

### 対処法

Laravel 8 では、コントローラのアクション名をフルパスで書く必要がある。

```
Route::get('hello', 'App\Http\Controllers\HelloController@index');
```

## 終わりに

読み進めていくうちにまた動かないコードが出てきたら追記する予定。
