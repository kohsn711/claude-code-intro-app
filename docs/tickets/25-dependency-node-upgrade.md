# 25 Node.js・依存関係更新

> ステータス: 未着手

## 目的

Vercel、ローカル、CIをNode.js 24へ統一し、既知のhigh判定の依存脆弱性を解消する。

## 対象

- Next.jsと関連パッケージの互換性を公式文書で確認して更新する
- `next`、`postcss`、`sharp`、`nanoid`、`ws`のaudit結果を解消または評価する
- 4ロールのログイン、主要機能、権限境界を再確認する

## 完了条件

- [ ] 開発端末でNode.js 24を使用している
- [ ] `npm audit --omit=dev`のhigh判定が解消、または残存リスクを記録している
- [ ] lint、型チェック、ビルドが成功する
- [ ] Previewで主要回帰確認が完了する
- [ ] Vercel ProductionがNode.js 24で正常稼働する

## 進捗ログ

- 2026-08-12: 最優先課題として起票。実装は別PRで行う。
- 2026-08-12: 妥当性監査時のローカルはNode.js 22.20.0。`.nvmrc`、`package.json`、CIは24を指定済みだが、Node.js 24での検証は未実施。過去のaudit記録にmoderate 2件とhigh 5パッケージの差があるため、現在のlockfileをNode.js 24環境で再監査して結果を一本化する。
