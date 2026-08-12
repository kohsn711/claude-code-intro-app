# AGENTS.md

Codexを中心とするAIエージェント向けの最小ガイド。詳細は正本文書を参照する。

## 現状

「びーびー日記」は中高生の野球選手向け記録アプリ。MVPの主要機能は実装済みで、Vercel本番環境で4ロールの主要フローを確認済み。進捗の一次ソースは`docs/plan.md`と各`docs/tickets/*.md`。

**IMPORTANT:** ProductionとPreview、ローカルは当面同じSupabaseプロジェクトを使う。Previewとローカルではテスト専用アカウント・データだけを使用し、実利用データを変更しない。

**IMPORTANT:** 本番DBマイグレーション前と本番リリース前に、DBとStorageをセットでバックアップする。手順は`docs/operations/backup.md`。

**IMPORTANT:** Next.jsはこのバージョン固有の変更がある。Next.jsコードを変更する前に`node_modules/next/dist/docs/`の該当ガイドを読む。

### コマンド

| 用途 | コマンド |
| --- | --- |
| 開発 | `npm run dev` |
| lint | `npm run lint` |
| 型チェック | `npm run typecheck` |
| 本番ビルド | `npm run build` |
| 本番起動 | `npm run start` |

`package-lock.json`は必ずコミットし、CIでは`npm ci`を使う。

## 正本

| ファイル | 役割 |
| --- | --- |
| `docs/requirements.md` | プロダクト要件の唯一の正 |
| `docs/roadmap.md` | フェーズと役割分担の索引 |
| `docs/plan.md` | 優先順位、マイルストーン、リリースゲート |
| `docs/dev-workflow.md` | AI開発、レビュー、PR、リリース手順 |
| `docs/setup.md` | ローカル開発環境の構築 |
| `docs/design/` | アーキテクチャ、データモデル、ADR |
| `docs/operations/` | デプロイ、テスト、バックアップ、セキュリティ運用 |
| `docs/tickets/` | 個別作業。状態と進捗ログは各チケット内を正とする |
| `docs/meetings/` | 重要な決定、マイルストーン、本番運用変更の記録 |

## 役割分担

- Codex: 調査、設計詳細化、実装、検証、コードレビュー、関連文書更新
- ユーザー: 要件と優先順位の決定、実機確認、PR確認とマージ、本番サービスの外部設定
- コード変更で古くなる要件・設計・運用文書は同じPRで更新する。
- 複数工程または設計判断を伴う作業は、着手前にチケットを作り進捗ログへ1行記録する。
- 重要な技術・セキュリティ・データ設計の判断だけをADRに残す。
- 作業終了時は`$wrap-up`で状態をリポジトリへ残す。AIは明示依頼なしにコミットしない。

## ドメイン上の注意

- 記録は学生ごとに1日1件。カテゴリ未入力でも保存できる。
- 学生記録は本人、activeな親子リンクの保護者、所属チームの監督だけが閲覧できる。チームメイトには非公開。
- 監督は監督のコメント・リアクションだけ、保護者は保護者のものだけを閲覧し、学生は双方を閲覧する。
- 保護者は学生からの招待を起点にOTPで初回設定する。学生・監督アカウントは現状Supabase Dashboardで手動作成。
- 健康・ケガ情報は記録であり、医療判断として扱わない。
- 表示名は「びーびー日記」。本番URL、GitHub名、`package.json`名、Supabase内部識別子は当面変更しない。

## セキュリティ

- Server Actionでも認証・認可を省略しない。RLSを最終防衛線にする。
- サーバーではSupabase Authの`getClaims()`を使い、`getSession()`を信頼しない。
- `SUPABASE_SERVICE_ROLE_KEY`、DBパスワード、SMTP資格情報、テストアカウント情報をコードや文書へ書かない。
- service role keyはClient Componentや`NEXT_PUBLIC_*`へ絶対に渡さない。
- 認証、権限、個人情報を変更するPRは通常レビューに加えセキュリティ観点でレビューする。

## 実装方針

- Server Componentを基本とし、Client ComponentはブラウザAPIや操作状態が必要な最小単位にする。
- DB操作はサーバー側へ寄せる。独立した取得は可能なら並列化する。
- `params`と`searchParams`はPromiseとして`await`する。
- UIはスマホファーストで、最低限の記録を約1分で完了できることを優先する。
- 依頼外のリファクタ、将来要件向け抽象化、過剰な必須入力を追加しない。
- DBは`snake_case`、TypeScriptは`camelCase`、コンポーネントは`PascalCase`、URLは`kebab-case`。
- 文書とユーザー向け文章は日本語、コード識別子は英語を基本とする。
