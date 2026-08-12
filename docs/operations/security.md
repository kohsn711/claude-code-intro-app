# セキュリティ運用

## 実装済みの防御

- Supabase Authの`getClaims()`によるセッション確認
- Proxy、Server Component、Server Actionのロール確認
- 記録、目標、親子リンク、チーム、反応、通知、コンテンツのRLS
- ロール昇格防止、チーム所属制限、通知なりすまし防止のDB trigger / RPC
- 監督と保護者のコメント・リアクション相互非表示
- 基本セキュリティヘッダー
- `.env*`、AIローカル設定、秘密情報のGit除外

## 外部アカウント

2026-08-12時点:

| 対象 | 状態 | 実利用開始条件 |
| --- | --- | --- |
| GitHub | 2FA未設定 | 有効化する |
| Vercel | 2FA inactive | 有効化する |
| Supabase | MFA 0 apps configured | 有効化する |
| GitHub repository | Public | Private化し、自動デプロイを再確認する |

## 秘密情報

- テストアカウント情報は現在Notionにある。Bitwardenへ移し、Notionから削除する。
- ProductionにはSensitiveな`SUPABASE_SERVICE_ROLE_KEY`の変数項目があるが、値の設定有無は未確認。現行コードは未使用で、削除判断は保留。
- service role keyは、将来の管理者専用Auth Admin APIでのみ使用候補となる。サーバー側、admin確認、監査ログを必須とする。
- Bitwardenは未導入。導入後はSMTP資格情報、DBパスワード、バックアップ暗号化キーも移行する。
- GitHub Actionsのgitleaksで誤コミットを検知する。

## 依存関係

2026-08-09の`npm audit --omit=dev`でhigh判定5パッケージを確認した。

- `next`
- `postcss`
- `sharp`
- `nanoid`
- `ws`

Next.jsのProxy回避を含むため、機能改良より先に更新する。Dependabotは更新PRを作るが、自動マージせずAIレビューと手動確認を行う。

## 未成年者と個人情報

- 氏名、学年、食事、体調、ケガ、親子関係を扱う。
- 利用規約、プライバシーポリシー、保護者同意方法、退会後の保持・削除を実利用開始前に専門家確認する。
- 問い合わせ窓口を実利用開始前に設ける。

## Storage公開範囲

- `content-thumbnails`は現在public bucketで、画像URLを知る利用者は未認証でも画像本体を閲覧できる。
- コンテンツ一覧・本文のロール制御とは別に、サムネイルを公開のまま扱うか実利用開始前に決定する。

## 監視

- MVP中はVercel Logsを手動確認する。
- Sentry等の外部エラー監視、Web Analytics、Speed Insightsは将来対応。
- 利用状況は当面、アプリDBの記録データから把握する。
