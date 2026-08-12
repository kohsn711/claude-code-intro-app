# 開発ワークフロー

Codexを中心に開発を継続するための正本。何を作るかは[実行計画](plan.md)、個別作業は[tickets](tickets/README.md)を参照する。

## 基本方針

- Codexが調査・実装・検証・関連文書更新を担当する。
- ユーザーが要件決定、実機確認、PR確認・マージ、外部サービス設定を担当する。
- チャット履歴ではなくリポジトリ内の正本文書とチケットで状態を引き継ぐ。
- コード変更で古くなる文書は同じPRで更新する。

## 作業単位

- 文言修正などの小さな変更はチケットなしでもよい。
- 複数工程、設計判断、認証・権限・データ変更を伴う作業は`docs/tickets/`へ起票する。
- チケット着手時にステータスを`in progress`へ変更し、進捗ログへ日付・現在地・次の作業を記録する。
- 重要な技術・セキュリティ・データ設計の決定は`docs/design/adr/`へ残す。
- 重要な要件決定、マイルストーン完了、本番運用変更だけを`docs/meetings/`へ残す。

## 実装ループ

1. 要求を`requirements.md`と`plan.md`に照らしてスコープ判断する。
2. 必要ならチケットを起票し、進捗ログへ着手を記録する。
3. Next.js変更前に`node_modules/next/dist/docs/`の該当ガイドを読む。
4. 実装し、関連する要件・設計・運用文書を同時に更新する。
5. ローカルでlint、型チェック、ビルドを実行する。
6. コード差分をCodexでレビューする。認証・権限・個人情報の変更はセキュリティ観点も追加する。
7. ユーザーがpushし、`develop`から`main`へのPRを作成する。
8. CIとVercel Previewを確認する。
9. ユーザーが`main`へマージし、Production Deploymentと変更機能を確認する。
10. 実機・本番確認結果をチケットまたはmeetingへ記録する。

## 検証

```bash
npm run lint
npm run typecheck
npm run build
```

MVP中は自動テストを導入せず、変更機能と主要な認証・権限境界を手動確認する。詳細は[テスト運用](operations/testing.md)。

## ブランチ・PR

- 開発ブランチ: `develop`
- Production Branch: `main`
- `develop`へのpushでVercel Previewを自動作成する。
- `main`へのマージでVercel Productionを自動デプロイする。
- 個人開発のためPRは自己レビューする。
- GitHub FreeのPrivateリポジトリではBranch Protectionを使えないため、`main`へ直接pushしない運用で守る。
- PRマージ前にCI、Preview、変更機能、主要な認証・権限を確認する。

## Previewの扱い

- 当面はProduction、Preview、ローカルで同じSupabaseプロジェクトを使用する。
- Previewには`NEXT_PUBLIC_SUPABASE_URL`と`NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY`だけを設定する。
- Previewでは4ロールのテスト専用アカウントとテストデータだけを使用する。
- PreviewはVercel Standard Protectionで保護する。
- `SUPABASE_SERVICE_ROLE_KEY`をPreviewへ設定しない。

## AIレビュー

- コード変更はPR前に差分レビューする。
- 認証、認可、RLS、入力検証、秘密情報、個人情報を触る場合は、その観点で全件レビューする。
- docsだけの変更ではコードレビューを省略できる。
- フェーズ境界で必要なら、会話文脈を持たない新規セッションに要件整合・文書とコードの乖離・コード品質をレビューさせる。

## セッション終了

作業終了時は`$wrap-up`を使用する。報告には次を含める。

- 作業内容と検証結果
- 本番反映の要否
- セッション切替の推奨または継続可
- 次セッション用プロンプト案
- `bbnote_NNN-topic`形式のセッション名案（例: `bbnote_024-cold-start`）

Codexは明示依頼なしにコミットしない。
