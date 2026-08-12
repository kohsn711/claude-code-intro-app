# びーびー日記

中学生から高校生の野球選手向け記録アプリです。学生が練習、体づくり、食事、体調、ケガ、振り返りを記録し、保護者と監督が閲覧・リアクション・コメントで継続を支援します。

## 現在の状態

- Vercelへデプロイ済み: <https://baseball-note-app.vercel.app>
- 学生、監督、保護者、管理者の主要フローを本番環境で確認済み
- 現時点の利用者はテストアカウントのみ
- PWA Phase 1を実装し、iPhone Chromeでホーム画面追加とstandalone起動を確認済み
- 自動テストは未導入。MVP期間中は変更機能と主要な認証・権限を手動確認

## 技術スタック

- Next.js 16 App Router / React 19 / TypeScript
- Supabase Auth / PostgreSQL / Storage
- Tailwind CSS v4
- Vercel

## 開発

Node.js 24を使用します。詳細は[初回セットアップ](docs/setup.md)を参照してください。

```bash
npm ci
npm run dev
npm run lint
npm run typecheck
npm run build
```

## ドキュメント

- [要件](docs/requirements.md)
- [ロードマップ](docs/roadmap.md)
- [実行計画](docs/plan.md)
- [開発ワークフロー](docs/dev-workflow.md)
- [設計](docs/design/architecture.md)
- [運用](docs/operations/deployment.md)
- [チケット](docs/tickets/README.md)

AIエージェントは最初に[AGENTS.md](AGENTS.md)を確認してください。
