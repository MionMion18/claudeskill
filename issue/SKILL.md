---
name: issue
description: GitHub Issueの作成とProjectsへの追加を対話的に行うスキル。「issueを作成して」「タスクを追加して」「○○のissueを立てて」のようにユーザーがGitHub issueの作成を依頼したときに使用する。締切、担当者、追加先プロジェクトを対話的に確認する。
---

# GitHub Issue 作成スキル

## ワークフロー

### 1. リポジトリの特定

現在のディレクトリの `git remote -v` からowner/repoを取得する。取得できない場合はユーザーに確認する。

### 2. Issue内容の整理

ユーザーの入力からissueのタイトルと概要を整理する。情報が足りない場合のみ確認する。

### 3. 詳細を対話的に確認

AskUserQuestionツールを使い、以下を **1回のAskUserQuestion呼び出しで** まとめて確認する:

- **締切**: 「今日」「今週中」「来週」「指定なし」から選択
- **担当者**: リポジトリのコラボレーターから選択、または「指定なし」
- **プロジェクト**: `gh project list --owner <owner>` で取得したプロジェクト一覧から選択、または「追加しない」

ユーザーが最初のメッセージで既にこれらの情報を提示している場合は、その情報をそのまま使い、確認を省略する。

### 4. Issue作成

`mcp__github__create_issue` でissueを作成する。

- title: 整理したタイトル
- body: 概要、締切、タスクチェックリストを含むMarkdown
- assignees: 指定があれば設定
- labels: 内容に応じて適切なラベルを設定

### 5. プロジェクトへの追加

プロジェクトが選択された場合、以下をBashで実行:

```
gh project item-add <PROJECT_NUMBER> --owner <OWNER> --url <ISSUE_URL>
```

### 6. 結果報告

作成したissueのタイトル、URL、追加先プロジェクトをユーザーに簡潔に報告する。
