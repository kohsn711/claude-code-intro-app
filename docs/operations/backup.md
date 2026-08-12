# バックアップ・復元運用

## 現状

- Supabase Freeプランのため日次自動バックアップはない。
- 現在の利用者はテストアカウントだけ。
- DBバックアップにはStorage内の画像ファイル本体が含まれない。

## 取得タイミング

実利用開始前は次のタイミングで手動取得する。

- 本番DBマイグレーション前
- 本番リリース前

実利用開始後は定期実行とSupabase有料プランを再検討する。

## 対象

- Supabase PostgreSQLのスキーマとデータ
- Supabase Storageの`content-thumbnails`バケット

DBとStorageは同じバックアップ単位で取得する。

## 保管

```text
baseball-note-backups/
  YYYY-MM-DD_HHMM_<commit>/
    database/
    content-thumbnails/
    manifest.txt
```

- バックアップを暗号化アーカイブにする。
- Bitwarden導入後、暗号化キーをそこで管理する。Bitwardenは現在未導入であり、暗号化・鍵管理手順の確定前に運用開始済みと扱わない。
- 暗号化済みファイルをOneDriveへ保存する。
- バックアップや暗号化キーをGit、Notion、通常の共有フォルダへ置かない。
- `manifest.txt`には取得日時、Git commit、migration番号、対象バケット、復元確認状態を書く。秘密情報は書かない。

## 未整備の作業

- Supabase CLIによるDB dumpコマンドを確定する。
- Storageの安全な一括ダウンロード手順を確定する。
- 暗号化形式と復号手順を確定する。
- Bitwardenを導入し、暗号化キーの登録・復旧方法を確定する。
- 新しいSupabaseプロジェクトへの復元を1回リハーサルする。
- 目標復旧時点と許容停止時間を実利用開始前に決める。

手順が確定するまでは、バックアップを取得したつもりでリリースゲートを完了にしない。
