---
name: weight-training-log
description: Record weight training exercises, sets, reps, and durations into daily JSON files. Use when user asks to record today's workout menu, log exercises, or save training records.
---

# ウェイトトレーニング記録スキル

## 概要

`C:\Users\hsmtk\proton\weight-training-log` に日別JSONファイルでトレーニング記録を保存する。

## 記録形式

1日のファイルを `YYYY-MM-DD.json` で作成する。

```json
{
  "date": "2026-09-28",
  "exercises": [
    {
      "name": "exercise name",
      "weight": "8kg",
      "sets": [
        {"reps": 10},
        {"reps": 10},
        {"reps": 10}
      ]
    }
  ]
}
```

時間系エクササイズ（farmers walk等）は `duration` を使用する：

```json
{
  "name": "farmers walk",
  "weight": "12.5kg per hand",
  "sets": [
    {"duration": "1min"},
    {"duration": "1min"},
    {"duration": "1min"}
  ]
}
```

## ワークフロー

1. 日付を確認する（未指定の場合は今日）。
2. 記録前に曖昧な点を確認する：
   - 重量：farmers walk等の片手あたりか合計か
   - エクササイズ名のスペル
   - セット数・回数・時間
3. 既存ファイルの有無を確認し、上書き時に確認する。
4. `C:\Users\hsmtk\proton\weight-training-log/YYYY-MM-DD.json` に書き込む。
5. 記録内容をユーザーに確認する。
