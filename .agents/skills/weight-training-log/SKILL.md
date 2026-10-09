---
name: weight-training-log
description: Record weight training exercises, sets, reps, and durations into daily JSON files. Use when user asks to record today's workout menu, log exercises, or save training records.
---

# ウェイトトレーニング記録スキル

## 概要

環境変数 $WEIGHT_TRAINING_LOG フォルダに日別JSONファイルでトレーニング記録を保存する。

## 言語規約

記録ファイル（`YYYY-MM-DD.json`）に書き込む値はすべて英語で統一する。日本語表記は使用しない。

- `name`: 英文の種目名（`dumbbell shoulder press`、`farmers walk`）
- `weight`: 数値＋英文単位（`80kg`、`bodyweight`、`12.5kg per hand`）
- `duration`: 数値＋英文単位（`1min`、`70 secs`）

表記は半角英数字と半角記号のみとする。

ユーザーが日本語で入力した場合は、記録前に標準的な英文種目名へ変換し、変換結果をユーザーに確認する。
書き込み前に日本語（漢字・かな）が含まれないことを検証し、混在する場合は記録を中止してユーザーに確認する。

## 記録形式

1日のファイルを `YYYY-MM-DD.json` で作成する。

```json
{
  "date": "2026-09-28",
  "exercises": [
    {
      "name": "exercise name",
      "weight": "8kg",
      "reps": [10, 10, 10]
    }
  ]
}
```

時間系エクササイズ（farmers walk等）は `duration` を使用する：

```json
{
  "name": "farmers walk",
  "weight": "12.5kg per hand",
  "duration": ["1min", "1min", "1min"]
}
```

サンプルファイル: @.agents\skills\weight-training-log\sample.json

## 入力フォーマット

ユーザーは一括入力で複数種目を一度に伝える。

**形式：** `種目名 重量 セット数×レップ数` を `、` または `,` で区切る

**例：**
```
ベンチプレス 80kg 3セット×10回、バーベルロー 50kg 3セット×12回
```

- セット数×レップ数の区切りは `×` または `x`
- 重量は文字列としてそのまま格納（単位分離なし）
- 重量の指定がない種目は自重種目とみなし、`weight` に `bodyweight` を記録する（ユーザーへの確認は不要）
- 時間系エクサ尺寸は `duration` 配列で記録（例: `["1min", "1min", "1min"]`）

## ワークフロー

1. 日付を確認する（未指定の場合は今日）。
2. 既存ファイルの有無を確認し、上書き時に確認する。
3. ユーザーから一括入力を受け付ける。
4. 入力をパースし、各エクササイズについて種目名・重量・セット数・レップ数を抽出する。
5. バリデーション：セット数・レップ数が正の整数であることを確認。失敗時は再入力を求める。
6. パース結果をユーザーに確認してもらう。
   - レップ系は `reps: [10, 10, 10]` 形式
   - 時間系は `duration: ["1min", "1min", "1min"]` 形式
7. `YYYY-MM-DD.json` に記録する。
8. 記録内容をユーザーに確認する。