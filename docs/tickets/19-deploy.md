# 19 Vercelデプロイ

> ステータス: 一部完了（Production稼働、Preview機能確認は準備中）

## 概要

Next.jsアプリをVercelへデプロイし、Productionを稼働させた。PreviewはDeploymentの自動生成と`Ready`状態まで確認済みで、公開環境変数の設定とアプリの機能確認は未実施。現行設定とリリース手順は[デプロイ運用](../operations/deployment.md)を正とする。

## 確認済み

- [x] GitHubリポジトリとVercelプロジェクトを連携
- [x] Production Branchを`main`に設定
- [x] `develop`へのpushでPreviewを自動作成
- [x] `main`へのマージでProductionを自動作成
- [x] ProductionへSupabase URLとpublishable keyを設定
- [x] Supabase Site URLとRedirect URLsを本番URLへ修正
- [x] Productionで4ロールのログイン、データ読み書き、主要権限を確認
- [x] VercelのProductionとPreview Deploymentが`Ready`になることを確認
- [x] HTTPS配信を確認

## 保留・後続

- PreviewへSupabase URLとpublishable keyを設定する
- GitHubリポジトリのPrivate化後に自動デプロイを再確認する
- Productionのservice role key変数項目は、値の設定有無を確認できておらず、削除判断を保留している

## 進捗ログ

- 2026-08-12: Vercel画面と本番動作を再確認し、実態へ更新。
