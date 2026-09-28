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

## 入力フォーマット

ユーザーは一括入力で複数種目を一度に伝える。

**形式：** `種目名 重量 セット数×レップ数` を `、` または `,` で区切る

**例：**
```
ベンチプレス 80kg 3セット×10回、バーベルロー 50kg 3セット×12回
```

- セット数×レップ数の区切りは `×` または `x`
- 重量は文字列としてそのまま格納（単位分離なし）
- 時間系エクササイズは `sets` 内を `{"duration": "Xmin"}` で記録

## ワークフロー

1. 日付を確認する（未指定の場合は今日）。
2. 既存ファイルの有無を確認し、上書き時に確認する。
3. ユーザーから一括入力を受け付ける。
4. 入力をパースし、各エクササイズについて種目名・重量・セット数・レップ数を抽出する。
5. バリデーション：セット数・レップ数が正の整数であることを確認。失敗時は再入力を求める。
6. パース結果をユーザーに確認してもらう。
7. `YYYY-MM-DD.json` に記録する。
8. 記録内容をユーザーに確認する。