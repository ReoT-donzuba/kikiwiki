# Contributing

日本語・英語のどちらでも構いません。Issues and PRs in either Japanese or English are welcome.

## 何を歓迎するか / What's welcome

- **ルール・テンプレートの改善** — `wiki-config/` の書式や運用ルールが実運用で回らなかった話は特に価値があります。「使ってみたらこう詰まった」だけでもIssueにしてください。
- **プロンプトの改善** — `.github/prompts/` の3つの手順（知識追加・検索・Lint）が期待通り動かなかったケース。
- **他のAIエージェントへの対応** — Cursor、Cline、Codex など、新しい指示ファイル形式の追加。
- **英訳・和訳** — README以外のドキュメントの翻訳。

> Improvements to the rules and templates in `wiki-config/`, fixes to the three prompts in `.github/prompts/`, support for additional AI agents, and translations are all welcome.

## 何を送らないでほしいか / What not to send

- **あなた自身のノート。** `notes/` と `wiki/` は空のスターターとして維持します。サンプルノートは1件だけです。
- 個人名・顧客名・組織固有の情報を含む変更。

> Please don't contribute your own notes — `notes/` and `wiki/` stay empty by design, with exactly one sample note. Nothing containing personal, customer, or organization-specific information.

## 変更するときの注意 / Before you edit

- **ルールは共通ファイルを直す。** `CLAUDE.md` / `AGENTS.md` / `.github/copilot-instructions.md` は `wiki-config/ai-agent-rules.md` を参照するだけの薄い入口です。ルール本体はそちらを編集してください。
- **プロンプトの正本は `.github/prompts/`。** `.claude/commands/` はそこを参照するだけです。片方だけ直すと食い違います。
- **READMEは2言語ある。** 内容を変えたら `README.md`（英）と `README.ja.md`（日）の両方を更新してください。片方だけの更新なら、その旨をPRに書いてもらえれば構いません。
- メタデータのフィールド名（`knowledge_origin` / `evidence_level` / `status` / `portable`）は変更しないでください。プロンプト側が名前で一致させています。

> Edit the shared ruleset (`wiki-config/ai-agent-rules.md`), not the per-tool pointers. `.github/prompts/` is the source of truth for the procedures; `.claude/commands/` only references it. Keep `README.md` and `README.ja.md` in sync. Don't rename the metadata fields — the prompts match on them.

## PR の出し方 / Sending a PR

1. このリポジトリをフォークする
2. ブランチを切る（例: `fix/lint-prompt-orphan-check`）
3. 変更して push、Pull Request を開く

変更の意図を1〜2行で書いてもらえれば十分です。テストもビルドもありません（中身はMarkdownだけです）。

> Fork, branch, push, open a PR. One or two lines on intent is enough. There is no build and no test suite — it's Markdown all the way down.

## ライセンス / License

PRを送った時点で、その内容が [MIT License](LICENSE) の下で配布されることに同意したものとみなします。

> By submitting a PR you agree that your contribution is licensed under the [MIT License](LICENSE).
