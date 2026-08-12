# データモデル

## 主要リレーション

```text
auth.users 1--1 profiles
profiles   N--N teams               via team_members
profiles(parent) N--N profiles(student) via parent_child_links
profiles(student) 1--N daily_records
daily_records 1--N practice_entries / training_entries
daily_records 1--1 meal_records / condition_records / injury_records / reflection_records
profiles(student) 1--N goals
daily_records 1--N reactions / comments
profiles 1--N notifications
profiles(admin) 1--N contents
```

## 記録

`daily_records`を親とし、`(student_id, record_date)`を一意にする。

| カテゴリ | テーブル | 内容 |
| --- | --- | --- |
| 練習 | `practice_entries` | 素振り、ティー、キャッチボール、投球、守備、走塁、メモ |
| 体づくり | `training_entries` | ランニング、ダッシュ、腕立て、腹筋、スクワット、ストレッチ、メモ |
| 食事 | `meal_records` | 朝・昼・夕・補食、水分、メモ |
| 体調 | `condition_records` | 睡眠、起床・就寝、体重、体調 |
| ケガ | `injury_records` | 痛み、部位、レベル、練習への影響、メモ |
| 振り返り | `reflection_records` | できたこと、課題、明日、気分 |

未入力カテゴリがあっても保存できる。

## 権限関係

- `team_members`: ユーザーとチーム、チーム内ロールを結ぶ。学生は複数所属可能。
- `parent_child_links`: `pending`は招待管理、`active`だけを記録アクセス権に使う。
- `goals`: 学生本人が管理し、所属チームの監督は閲覧だけ可能。
- `reactions` / `comments`: 監督または保護者が関係する学生の記録へ作成する。
- `notifications`: 対象ユーザー本人だけが閲覧・既読化する。
- `contents`: 公開状態と`for_student / for_parent / for_coach`の両方で閲覧を制御する。

## スキーマ変更

- SQLは`supabase/migrations/`へ連番で追加する。
- 過去のmigrationを変更せず、新しいmigrationで前進させる。
- 本番適用前にDBとStorageをバックアップする。
- RLS、制約、RPC、Storage policyを含めてレビューする。
