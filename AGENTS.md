# AGENTS.md

このリポジトリは、論理的思考のフィードバックを受けるためのスキルとエージェントの置き場。Codex・Kiro・Claude Code で共通して使う。

## 文章・考えのレビューを頼まれたら

報告・相談・提案の下書き、課題の分解、「検討してください」と言われた件の進め方などを見せられたら、`.agents/skills/logic-feedback/SKILL.md` を読み、その手順に従ってFBする。点数をつけ、`feedback-log/` に記録を残す。

## ファイルの配置

- スキル本体は `.agents/skills/logic-feedback/` だけにある。`.claude/skills/` と `.kiro/skills/` はそこへのシンボリックリンク
- 定義・採点基準・記録形式を変えるときは `.agents/skills/logic-feedback/` 以下を編集する
- エージェント定義は各ツール用に薄いラッパーとして置いている（`.claude/agents/`、`.codex/agents/`、`.kiro/agents/`）。中身はスキルを読ませるだけにして、内容を重複させない
- `feedback-log/` は個人の記録なので git 管理しない
