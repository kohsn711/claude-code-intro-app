# デプロイ運用

## 現在の環境

| 項目 | 設定 |
| --- | --- |
| 本番URL | <https://baseball-note-app.vercel.app> |
| Vercelプラン | Hobby |
| Production Branch | `main` |
| Preview | `main`以外のGitブランチ。`develop`へのpushによるDeployment生成と`Ready`実績を確認済み。アプリの機能確認は未実施 |
| Preview保護 | Vercel Standard Protection |
| Node.js | Vercel 24.x。ローカル・CIも24へ統一する |
| Supabase | `baseball-note` / Free / project ref `erhmthnfwwqlxkhcvyqt` |
| Supabase region | `ap-northeast-1`（Tokyo） |

## Vercel環境変数

| 変数 | Production | Preview | Development |
| --- | --- | --- | --- |
| `NEXT_PUBLIC_SUPABASE_URL` | 設定済み | 未設定。設定予定 | Vercel上は未設定 |
| `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY` | 設定済み | 未設定。設定予定 | Vercel上は未設定 |
| `SUPABASE_SERVICE_ROLE_KEY` | Sensitiveな変数項目は存在するが、値の設定有無は未確認。コード未使用で削除判断は保留 | 設定しない | 設定しない |

ローカルは`.env.local`を使う。当面、Production / Preview / localは同じSupabaseプロジェクトへ接続し、Production以外ではテスト専用アカウント・データだけを使う。

Previewの公開環境変数は未設定のため、現時点で確認できているのはDeploymentの自動生成と`Ready`状態まで。次回の機能改良から公開環境変数を設定し、Preview上で変更機能と主要な認証・権限を確認する。

## Supabase Auth URL

```text
Site URL:      https://baseball-note-app.vercel.app
Redirect URL: https://baseball-note-app.vercel.app/**
Redirect URL: http://localhost:3000/**
```

2026-08-12に誤って重複していた`.vercel.app`を修正済み。

## リリース手順

1. `develop`で変更する。
2. `npm run lint`、`npm run typecheck`、`npm run build`を実行する。
3. Codexで差分レビューする。認証・権限・個人情報変更はセキュリティ観点も確認する。
4. `develop`をpushし、Vercel Previewが`Ready`になることを確認する。
5. Previewで変更機能と主要な認証・権限をテスト専用アカウントで確認する。
6. `develop`から`main`へのPRを作り、GitHub Actions CIが成功することを確認する。
7. 本番リリース前バックアップを取得する。
8. ユーザーがPRを自己レビューして`main`へマージする。
9. Vercel Productionが`Ready`になることを確認する。
10. 本番で変更機能をスモーク確認し、結果をチケットへ記録する。

GitHub FreeのPrivateリポジトリではBranch Protectionを利用できないため、`main`へ直接pushしない運用で守る。

## DBマイグレーション

- `supabase/migrations/`のSQLを番号順にSupabase SQL Editorで手動適用する。
- 本番適用前にバックアップを取得する。
- 適用後、対象のテーブル・関数・RLSと正常系を確認する。
- ユーザーがSupabase SQLで、`0017_social_visibility_security.sql`までの適用結果として`get_record_reactions(uuid)`の存在を2026-08-09に確認済み。リポジトリから本番DBの適用状態は再検証できない。
- 将来Supabase CLIへ移行する場合、手動適用済みmigrationの履歴をrepairしてから`db push`を使う。

## ロールバック

- アプリだけの問題はVercelで直前の正常Deploymentへ戻す。
- DB変更は安易に逆SQLを実行せず、影響範囲とバックアップを確認して前進migrationまたは復元を選ぶ。
- 復元は停止時間を伴うため、実利用開始前にリハーサルする。
