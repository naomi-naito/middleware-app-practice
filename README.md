# Laravel Middleware Starter

COACHTECH 教材 Tutorial 10-2「ミドルウェア ハンズオン演習」で作成した成果物です。
カスタムミドルウェアを使って、管理者のみアクセスできる「管理者専用ページ」を保護しました。

## 含まれているもの

- Laravel 10.x + Sail
- Fortify認証（ログイン / 登録 / ログアウト）
- Userモデル（is_adminカラム付き）
- テストユーザー（シーダー）
  - 管理者: admin@example.com / password
  - 一般: user@example.com / password
- AdminController
- 管理者ページ（/admin）

## 使用技術
- PHP 8.x
- Laravel 10.x
- カスタムミドルウェア
- Laravel Fortify（認証）
- Laravel Sail（Docker Compose）

## セットアップ

```bash
# クローン
git clone https://github.com/coachtech-material/laravel-middleware-starter.git middleware-app-sample
cd laravel-middleware-starter

# 依存関係インストール
docker run --rm \
  -u "$(id -u):$(id -g)" \
  -v "$(pwd):/var/www/html" \
  -w /var/www/html \
  -e COMPOSER_CACHE_DIR=/tmp/composer_cache \
  laravelsail/php82-composer:latest \
  composer install

# 環境ファイル作成
cp .env.example .env

# Sail起動
./vendor/bin/sail up -d

# アプリケーションキー生成
./vendor/bin/sail artisan key:generate

# マイグレーション・シーダー実行
./vendor/bin/sail artisan migrate --seed
```

## 動作確認

1. http://localhost にアクセス
2. admin@example.com / password でログイン
3. 「管理者ページにアクセス」をクリック → 表示される
4. ログアウトして user@example.com / password でログイン
5. 「管理者ページにアクセス」をクリック → 表示される（ミドルウェア未実装のため）

## 完成後の動作

- admin@example.com でログイン → /admin アクセス → ✅ 表示される
- user@example.com でログイン → /admin アクセス → ❌ 403エラー

## 学んだこと
- ミドルウェアが Request と Response の間で処理を介在させる仕組み
- sail artisan make:middleware でのミドルウェア作成と Kernel.php での登録
- ルートへのミドルウェア適用方法（->middleware('check_admin')）