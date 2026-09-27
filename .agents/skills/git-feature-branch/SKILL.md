---
name: git-feature-branch
description: Create a feature branch, stage changes, commit with conventional commit format, push, and open a PR. Use when user asks to create a feature branch, start a new branch, or open a pull request.
---

# Git Feature Branch Skill

## 概要

ブランチ作成からPR作成までを一気通貫で行う。

## ワークフロー

### 1. ブランチ名の生成

AIが自律的に生成する。以下のルールに従う：

- 形式: `<type>/<slug>`
  - `type`: `feature` / `fix` / `refactor` / `chore` / `docs` / `test` / `perf` / `ci`
  - `slug`: 英小文字、数字、ハイフンのみ。複数単語は `-` で結合
- チケット番号が明示的に指定された場合は、slug の先頭に `<issue-number>-` を付与
- 既存ブランチと重複しないか確認し、重複する場合は番号付で差別化

### 2. ブランチ作成

```bash
git checkout -b <branch-name>
```

### 3. 変更のステージ

```bash
git add -A
```

特定のファイルのみステージする場合は、指定されたファイルをステージする。

### 4. コミットメッセージの生成

AIが自律的に生成する。conventional commit 形式に従う：

```
<type>(<scope>): <description>

<body>
```

- **type**: 変更内容から判断（`feat` / `fix` / `refactor` / `chore` / `docs` / `test` / `perf` / `ci`）
- **scope**: 変更が影響するモジュールやファイル群（任意）
- **description**: 変更の要約（日本語、簡潔に）
- **body**: 詳細や背景（任意）

### 5. コミット実行

```bash
git commit -m "<commit-message>"
```

### 6. プッシュ

```bash
git push -u origin <branch-name>
```

### 7. PR作成

`gh pr create` を使用し、以下の要素で自動生成：

- **タイトル**: コミットメッセージの `<type>(<scope>): <description>` を日本語で
- **本文**: 変更内容の要約、関連する背景、確認事項

```bash
gh pr create --title "<title>" --body "<body>"
```

## 注意事項

- ユーザーがブランチ名やメッセージを指定した場合は、それを優先する
- 既存ブランチの存在確認は push 時に実施する
- `gh` CLI が未インストールの場合は、PR作成を警告してリモートURLを案内する
- コミット前の変更内容が空の場合は中止して警告する