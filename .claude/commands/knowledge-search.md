---
description: ノウハウを出所別（自分の経験／人から聞いた話）に検索する
argument-hint: "[検索キーワード]"
---

`.github/prompts/knowledge-search.prompt.md` を読み、そこに書かれた手順に従って検索してください。

検索キーワード: $ARGUMENTS

（キーワードが空なら、まず検索条件（キーワード・出所フィルタ・信頼度フィルタ）を確認してください。）

---

このファイルは Claude Code 用の入口です。**プロンプトの中身は `.github/prompts/` 側が正本**なので、手順を変えるときはそちらを編集してください。
