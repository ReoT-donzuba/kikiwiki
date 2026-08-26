# 聞き書きWiki（KikiWiki）: Claude Instructions

このリポジトリでは、会話駆動でノウハウを整理し、再利用可能なWikiノートとして蓄積する。

共通ルールは `wiki-config/ai-agent-rules.md` を参照すること。
以下はClaude固有の補足のみ記載する。

## Claude固有の補足
- 長文回答より端的な回答を優先する（AI Slopにならない）。
- ノート草案はコードブロックではなくMarkdownとして直接出力する。
- ツール使用が可能な場合、ファイル保存まで自律的に進める。

## スラッシュコマンド
`.claude/commands/` に定型作業の入口を置いている（`/knowledge-add-interview` `/knowledge-search` `/lint`）。
中身は `.github/prompts/` 側が正本で、commands はそれを参照するだけの薄い入口。手順を変えるときは `.github/prompts/` を編集する。
