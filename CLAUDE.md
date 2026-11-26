# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 概要

投稿を一覧・編集・削除できる小さな Laravel 10 アプリ（教材用）。Tutorial 13〜15 でこのアプリ 1 本を題材に、設計・実装・仕組み化を進める。リポジトリ内の文書・コメント・コミットメッセージは日本語。

## コマンド

すべて Laravel Sail（Docker）経由で実行する。PHP / MySQL はコンテナ側にあり、ホストの `php` は前提にしない。

```bash
./vendor/bin/sail up -d                       # 起動
./vendor/bin/sail down                        # 停止
./vendor/bin/sail artisan migrate:fresh --seed  # DB を作り直して練習用データを入れる
./vendor/bin/sail artisan test                # 全テスト
./vendor/bin/sail artisan test --filter=テスト名  # 1 件だけ実行
./vendor/bin/sail artisan test tests/Feature/ExampleTest.php
./vendor/bin/sail pint                        # コード整形（Laravel Pint）
./vendor/bin/sail npm run dev                 # Vite
```

- テストは MySQL の `testing` データベースを使う（`phpunit.xml`。Sail の MySQL コンテナ初回起動時に自動で作られる）。SQLite ではない。
- 初回起動直後の `migrate` で「Connection refused」が出たら、MySQL の起動待ち。少し待って再実行する。

## アーキテクチャ

- **認証は Laravel Fortify**。ログイン・登録・ログアウト・パスワードリセットのルートは Fortify が提供するので `routes/web.php` には無い。ビューの割り当ては `app/Providers/FortifyServiceProvider.php`（`auth/login`, `auth/register`）、ユーザー作成処理は `app/Actions/Fortify/`。ログイン後の遷移先は `config/fortify.php` の `home`（`/posts`）。
- **投稿機能**は `PostController` の index / edit / update / destroy のみで、すべて `auth` ミドルウェア配下。投稿の新規作成ルートはまだ無い。
- **認可は `PostPolicy`**（投稿者本人だけが update / delete 可）。`AuthServiceProvider` の `$policies` は空で、ポリシーは命名規約による自動検出で `Post` に結び付いている。コントローラーは各アクションで `$this->authorize()` を呼び、他人の投稿は 403。ビュー側でも編集・削除ボタンを出し分けている。
- **データ**: `users` 1—* `posts` *—1 `categories`。ファクトリーは `UserFactory` のみ（Post / Category には無い）。
- **シーダー**（`DatabaseSeeder`）はカテゴリ 3 件と検証用アカウント 2 人（`usera@example.com` / `userb@example.com`、パスワードはどちらも `password`）、投稿各 2 件を入れる。検証用アカウントのメールとパスワードは変更しないこと（教材が前提にしている）。

## ドキュメント用フォルダ

- `docs/13-x/`: 学習者が Tutorial 13 の各章で作る設計書（Markdown と draw.io）。
- `answers/13-x/`: 設計書の解答例。アプリのコードとは無関係で、機能追加時に読む必要はない。ファイル名の接頭辞は `post-`（このアプリ）、`cafe-`（章末演習のカフェアプリ）、`register-`（会員登録のテスト設計）。
