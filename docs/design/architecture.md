# アーキテクチャ

## 概要

Next.js App RouterからSupabase Auth / PostgreSQL / Storageを利用する。独立したAPIサーバーは置かず、読み取りはServer Components、更新はServer Actionsを中心に実装する。

```text
Browser / PWA
  -> Vercel / Next.js App Router
       -> Supabase Auth
       -> Supabase PostgreSQL + RLS
       -> Supabase Storage
```

## ディレクトリ

```text
app/
  (auth)/       共通ログイン
  (student)/    学生ルート
  (coach)/      監督ルート
  (parent)/     保護者ルート
  (admin)/      管理ルート
components/     再利用UI
lib/            サーバー側ドメイン処理と共通定数
utils/supabase/ Supabaseクライアント
supabase/migrations/ DB・RLS・RPC・Storage変更
docs/           要件、設計、運用、チケット、記録
proxy.ts        セッション更新と保護ルート入口
```

## 認証・認可

- ProxyはSupabase Authの`getClaims()`でセッションを確認・更新する。
- 通常ページは`requireRole()`でアプリ側のロールを確認する。
- Server Actionはリクエストを直接送られる前提で認証・認可を再確認する。
- DBではRLS、SECURITY DEFINER関数、制約を最終防衛線にする。
- service role keyは現行アプリ処理では使用しない。

## データ取得

- Server Componentでデータを取得し、Client Componentには表示・操作に必要な値だけ渡す。
- 記録とカテゴリ子テーブルはネストselectでまとめて取得する。
- 独立した取得は`Promise.all`または画面専用集約関数で並列化する。
- 個人データを静的生成や共有キャッシュへ載せない。

## 更新

- Server Actionsで入力検証、認証、本人・関係者確認を行う。
- 更新後は`revalidatePath()`で必要な画面を更新する。
- コメント・リアクション通知はDBトリガーで生成し、任意ユーザーへの直接insertを許可しない。

## PWA

- manifest、静的アイコン、standalone表示、インストール案内までをPhase 1とする。
- Service Worker、オフラインキャッシュ、Web Pushは将来対応。
