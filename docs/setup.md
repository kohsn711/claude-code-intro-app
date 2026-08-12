# 初回セットアップ

## 前提

| ツール | バージョン |
| --- | --- |
| Node.js | 24.x |
| npm | Node.js 24同梱版 |

`.nvmrc`に合わせてNode.jsを切り替える。

2026-08-12の監査時点では既存ローカルシェルがNode.js 22.20.0だった。Node.js 24での再検証が終わるまでは[チケット25](tickets/25-dependency-node-upgrade.md)を未完了とする。

```bash
nvm install
nvm use
```

## インストール

```bash
npm ci
cp .env.example .env.local
```

`.env.local`へSupabase Dashboardの値を設定する。

```text
NEXT_PUBLIC_SUPABASE_URL=...
NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY=...
```

- ローカルと本番は当面同じSupabaseプロジェクトを使う。
- ローカルではテスト専用アカウント・データだけを使用する。
- `SUPABASE_SERVICE_ROLE_KEY`は現行機能に不要なので設定しない。
- Bitwardenは現在未導入。導入後に認証情報をNotionから移して削除し、それ以降はリポジトリやNotionへ保存しない。

## 起動

```bash
npm run dev
```

<http://localhost:3000>を開く。

## 検証

```bash
npm run lint
npm run typecheck
npm run build
```

## Supabaseマイグレーション

現在は`supabase/migrations/`のSQLを番号順にSupabase SQL Editorで手動適用する。最新の適用確認とバックアップ手順は[デプロイ運用](operations/deployment.md)と[バックアップ運用](operations/backup.md)を参照する。

Supabase CLIでの履歴管理は将来対応。既存SQLを手動適用済みのため、導入時はmigration履歴を現状へ合わせてから`db push`へ移行する。
