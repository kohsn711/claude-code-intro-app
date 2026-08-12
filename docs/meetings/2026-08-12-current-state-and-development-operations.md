# 2026-08-12 現状確認と開発運用方針

## 目的

開発再開に向け、リポジトリの文書と実環境を一致させ、今後の最小限のAI開発運用を決める。

## 決定

- サービス表示名を「びーびー日記」とする。URL、GitHub名、package名、Supabase内部名は当面維持する。
- Codexを中心に、要件と状態をリポジトリ内の文書・チケットで引き継ぐ。
- `docs/operations/`は参考テンプレート外のCodex独自提案として採用し、継続参照するデプロイ・テスト・バックアップ・セキュリティ運用をまとめる。
- `develop`からPRを作り、CI、Vercel Preview、手動確認後に`main`へマージする。
- MVP中は変更機能と主要な認証・権限を手動テストし、自動テストは後で再検討する。
- Previewは本番と同じSupabaseを使うが、テスト専用アカウント・データに限定する。
- 実利用開始前に依存関係、アカウント管理、認証メール、法務、バックアップ、2FAを対応する。

## 確認した事実

以下はユーザーがVercel・Supabase・GitHubの画面または本番アプリで確認した事項。リポジトリだけからは再検証できない。

- Vercel本番URLは<https://baseball-note-app.vercel.app>、Production Branchは`main`。
- `develop`へのpushでPreview、`main`へのマージでProductionが自動デプロイされ、いずれも`Ready`実績がある。Preview上の機能確認はまだ行っていない。
- Supabase Auth URLは本番URLとlocalhostへ修正済み。
- 本番で4ロールの主要機能、権限境界、iPhone ChromeのPWAをテスト済み。
- SupabaseはFreeプランで自動バックアップなし。VercelはHobby、PreviewはStandard Protection。
- GitHub、Vercel、Supabaseの2FA/MFAは未有効。GitHubリポジトリはPublic。
- Productionにはservice role keyの変数項目があるが、値の設定有無は確認できていない。現行コードでは使用していない。

## 未対応・次の作業

優先順位と実利用開始条件は[実行計画](../plan.md)、具体的な作業は[チケット索引](../tickets/README.md)を参照する。
