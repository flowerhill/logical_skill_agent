# logical_skill_agent

論理的思考のフィードバックを受けるためのスキルとエージェント。Claude Code・Codex・Kiro で使える。

報告・相談の下書きや検討メモを渡すと、MECE／論理的思考／クリティカルシンキングの観点で、上長役として構造面のレビューを返す。点数をつけ、記録を残すので、伸びている点と繰り返している指摘を後から振り返れる。文章を直してもらうためではなく、構造的に考えるクセを身につけるために使う。

## 構成

```
.agents/skills/logic-feedback/     # スキル本体（ここだけを編集する）
├── SKILL.md                       # 役割・手順・出力形式
└── references/
    ├── definitions.md             # 判定基準となる定義集
    ├── scoring.md                 # 採点基準
    └── log-format.md              # FB記録の形式・指摘タグ一覧

.claude/skills/logic-feedback      # → .agents/skills/logic-feedback へのシンボリックリンク
.kiro/skills/logic-feedback        # → 同上

.claude/agents/logic-reviewer.md   # Claude Code 用エージェント
.codex/agents/logic-reviewer.toml  # Codex 用エージェント
.kiro/agents/logic-reviewer.json   # Kiro 用エージェント

AGENTS.md                          # Codex・Kiro 向けの案内（CLAUDE.md からも読み込む）
feedback-log/                      # FBの記録（git 管理しない）
```

スキル本体は1か所にだけ置き、各ツールはそこを参照する。各ツールのエージェント定義は「スキルを読んで従う」だけの薄いラッパーなので、定義や採点基準を変えるときは `.agents/skills/logic-feedback/` だけを直せばよい。

## 使い方

このディレクトリを各ツールで開いて使う。

### Claude Code

```
/logic-feedback
（レビューしてほしい文章を貼る）
```

または「logic-reviewer でこの報告文を見て」。

### Codex

`.agents/skills/` のスキルが自動で読み込まれる。「この報告文に論理的にFBして」と頼むか、エージェントとして「`logic_reviewer` にこの報告文をレビューさせて」と頼む。

### Kiro

`.kiro/skills/` のスキルが自動で読み込まれる。「この報告文に論理的にFBして」と頼むか、カスタムエージェント `logic-reviewer` を選んで使う。

### 記録を振り返る

「FBの記録を見せて」「最近の傾向は？」と頼むと、`feedback-log/` から点数の推移とよく出る指摘をまとめて返す。

## 見てもらえるもの

| 種類 | 例 | 主に見る点 |
|---|---|---|
| 伝える文章 | 報告、相談、説明、チャット文 | 結論が先にあるか、根拠とつながっているか |
| 分解・整理 | 課題の洗い出し、選択肢の列挙 | MECE |
| 主張・判断 | 提案、結論、原因分析 | 推論の型と妥当性、検証 |
| 進め方の相談 | 「検討してください」と言われたが何をすればいいかわからない | 問いの分解 |

## 点数

入力に関係する軸だけを1〜5で採点し、平均×20で100点満点の総合点を出す。各軸の点数の基準（アンカー）は `scoring.md` にある。

| 軸 | 何を見るか |
|---|---|
| 構造 | 結論が先にあり、根拠とのつながりが見えるか |
| MECE | 観点が一つで、具体レベルが揃い、抜け漏れダブりがないか |
| 推論 | 根拠から結論へ第三者がたどれるか |
| 検証 | 根拠を確かめているか、事実と解釈を区別しているか |
| 分解 | 「検討」をゴール→問い→必要な情報→成果物に具体化できているか |

## 記録

FBのたびに `feedback-log/YYYY-MM-DD-HHMM-<題材>.md` が1つできる。先頭のYAMLフロントマターに点数と指摘タグが入り、本文に入力とFBが残る。形式は `log-format.md` を参照。

業務の文章が入るので `.gitignore` で git 管理から外している。記録もリポジトリに残したい場合は `.gitignore` の該当行を消す。

## FBの方針

- 判定基準は `definitions.md` の定義。一般論ではなく自分で決めた定義に照らす
- 指摘は優先度順に最大3つ。該当箇所を必ず引用する
- 書き直した完成版は最初から出さず、自分で直すための問いを返す。「直した例を見せて」と言えば例を出す
- 内容の正誤ではなく構造（結論・根拠・つながり・分け方）を見る

## 他のプロジェクトから使う

| ツール | コピー先 |
|---|---|
| Claude Code | `~/.claude/skills/logic-feedback`、`~/.claude/agents/logic-reviewer.md` |
| Codex | `~/.agents/skills/logic-feedback`、`~/.codex/agents/logic-reviewer.toml` |
| Kiro | `~/.kiro/skills/logic-feedback`、`~/.kiro/agents/logic-reviewer.json` |

スキルはシンボリックリンクにしておくと、このリポジトリの変更がそのまま反映される。

```bash
ln -s "$PWD/.agents/skills/logic-feedback" ~/.claude/skills/logic-feedback
```

記録は作業中のプロジェクトの `feedback-log/` にできる。1か所にまとめたい場合は、そのプロジェクトの AGENTS.md や CLAUDE.md に保存先を書いておく。

## Windows で使う場合

`.claude/skills` と `.kiro/skills` はシンボリックリンクなので、clone 前に `git config --global core.symlinks true` を設定し、開発者モードを有効にしておく。
