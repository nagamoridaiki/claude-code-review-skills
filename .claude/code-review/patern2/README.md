# code-review-skills

Claude Code 用のコードレビュースキル。PRの変更内容を分析し、関連するガイドラインを自律的に選択・読み込み、GitHub に pending review としてレビューコメントを投稿する。

## 構成

```
skills/code-review-skills/
├── SKILL.md              # スキル本体（レビュー手順・MCPツール指示）
└── guidelines/
    ├── common.md          # 全PR共通の横断チェック観点
    ├── rails.md           # Ruby on Rails / RSpec
    └── nextjs.md          # Next.js / React / TypeScript
```

## セットアップ

### 前提条件

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) がインストール済み
- GitHub MCP サーバーが設定済み

### シンボリックリンクで配置

```bash
REPO=/path/to/code-review-skills

# skills ディレクトリごとリンク
ln -sf "$REPO" ~/.claude/skills/code-review-skills
```

または、既存の skills ディレクトリがある場合:

```bash
REPO=/path/to/code-review-skills

# スキル単体をリンク
ln -sf "$REPO" ~/.claude/skills/code-review-skills
```

配置後の構成:

```
~/.claude/skills/
└── code-review-skills/    # → symlink to $REPO
    ├── SKILL.md
    └── guidelines/
```

## 使い方

Claude Code 上で以下のように呼び出す:

```
/code-review-skills <PR URL or PR番号>
```

スキルが自動で以下を実行する:

1. PR情報の取得（MCP経由）
2. レビュー用ワークツリーの作成
3. 変更ファイルに応じたガイドラインの選択・読み込み
4. 影響範囲の調査
5. 2回のレビューサイクル（網羅的リストアップ → 批判的に厳選）
6. pending review としてインラインコメントを投稿
7. ワークツリーのクリーンアップ

## カスタマイズ

### ガイドラインの追加

`guidelines/` に Markdown ファイルを追加し、`SKILL.md` のガイドラインテーブルに追記する。

```bash
vim guidelines/python.md
```

```markdown
# Python レビューガイドライン

- [ ] N+1クエリが発生していないか
- [ ] 型ヒントが付与されているか
...
```

`SKILL.md` のテーブルに追加:

```markdown
| `./guidelines/python.md` | Python、FastAPI、Django |
```

### ガイドラインの設計方針

- 各ファイルはチェックリスト形式（`- [ ]`）で記述する
- OK/NG の具体例を添えると、レビュー精度が向上する
- 1ファイル = 1技術領域に絞る（言語別・サービス別の2軸で分類可能）

## ライセンス

MIT