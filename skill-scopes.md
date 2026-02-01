# Claude Code スキルのスコープまとめ

## スキルの配置スコープ

スキルは4つのスコープに配置でき、同名スキルがある場合は上位が優先される。

| 優先順位 | スコープ | 配置場所 | 適用範囲 |
|---|---|---|---|
| 1（最高） | **Enterprise** | `managed-settings.json` | 組織全体 |
| 2 | **Personal（ユーザー）** | `~/.claude/skills/<name>/SKILL.md` | 全プロジェクト共通 |
| 3 | **Project** | `.claude/skills/<name>/SKILL.md` | そのプロジェクトのみ |
| 4（最低） | **Plugin** | `<plugin>/skills/<name>/SKILL.md` | プラグインのインストールスコープに依存 |

---

## 各スコープの詳細

### Enterprise

組織のIT管理者がデプロイする。個人ユーザーによる上書きは不可。

| OS | パス |
|---|---|
| macOS | `/Library/Application Support/ClaudeCode/managed-settings.json` |
| Linux/WSL | `/etc/claude-code/managed-settings.json` |
| Windows | `C:\Program Files\ClaudeCode\managed-settings.json` |

### Personal（ユーザー）

`~/.claude/skills/<skill-name>/SKILL.md` に配置。全プロジェクトで共通して使える。

### Project

プロジェクトルートの `.claude/skills/<skill-name>/SKILL.md` に配置。そのプロジェクト内でのみ有効。gitにコミットすればチームで共有できる。

モノレポの場合、ネストされたディレクトリからも自動検出される（例: `packages/frontend/.claude/skills/` と ルートの `.claude/skills/` の両方が有効）。

---

## プラグインのスコープ

プラグインはスキル・フック・MCPサーバーなどをまとめたパッケージ。インストール時にスコープを選択でき、そのスコープ内でのみプラグインに含まれるスキルが有効になる。

### プラグインのインストールスコープ

| スコープ | 設定ファイル | 有効範囲 |
|---|---|---|
| **user** | `~/.claude/settings.json` | 全プロジェクト |
| **project** | `.claude/settings.json` | そのプロジェクトのみ（git共有される） |
| **local** | `.claude/settings.local.json` | そのプロジェクトのみ（git共有されない） |
| **managed** | `managed-settings.json` | 組織全体（IT管理者が管理） |

同一プラグインが複数スコープにある場合の優先順位: **managed > local > project > user**

### プラグインのスキルの特徴

- スキル名に名前空間が付く: `/プラグイン名:スキル名`
- 他のスキルと名前が衝突しない
- スキル優先順位では最も低い（Enterprise > Personal > Project > Plugin）

---

## .skill ファイルについて

`.skill` ファイルはZIPアーカイブ形式。中に `<skill-name>/SKILL.md` が格納されている。

```
example.skill  (ZIP archive)
└── example/SKILL.md
```
