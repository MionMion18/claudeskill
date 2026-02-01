---
name: skillsdeploy
description: claudeskillリポジトリ内のスキルをPersonalまたはProjectスコープにデプロイする。「スキルをデプロイして」「deployして」のようにユーザーがスキルの配置を依頼したときに使用する。
---

# スキルデプロイ

## ワークフロー

### 1. スキル一覧の取得

`claudeskill/` ディレクトリ（このリポジトリのルート）内で `*/SKILL.md` パターンに一致するディレクトリを検索し、利用可能なスキルの一覧を作成する。`skillsdeploy` 自身は一覧から除外する。

### 2. デプロイ対象の選択

AskUserQuestion で、見つかったスキルの中からデプロイするスキルを選択してもらう。

### 3. スコープの選択

AskUserQuestion で、デプロイ先のスコープを確認する:

- **Personal**: `~/.claude/skills/<name>/SKILL.md` にコピー
- **Project**: ユーザーにプロジェクトのパスを入力してもらい、`<path>/.claude/skills/<name>/SKILL.md` にコピー

### 4. デプロイ実行

- 対象ディレクトリが存在しなければ `mkdir -p` で作成
- `SKILL.md` をコピー（`cp` コマンド）
- **Personal スコープの場合のみ**: 対応する `.skill` ファイル（`<name>.skill`）がリポジトリルートに存在すれば、`~/.claude/skills/` にコピー
- デプロイ先ディレクトリに、スキルディレクトリ全体（SKILL.md、.skill ファイル等）を含む `<name>.zip` を作成する（`zip` コマンド）

### 5. 結果報告

以下をユーザーに簡潔に報告する:
- デプロイしたスキル名
- スコープ（Personal / Project）
- コピー先のパス
- zip ファイルのパス
