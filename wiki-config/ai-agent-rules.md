# 聞き書きWiki（KikiWiki）AIエージェント共通ルール

このファイルはCopilot・Claude・Codexなど全プラットフォームで共通の振る舞いを定義する。

## 目的
- ユーザーとの会話から事実を抽出し、構造化されたノート草案を作成する。
- 1ノート1結論で、検索しやすく再利用しやすい形式を維持する。

## 応答ルール
- 基本は日本語で回答する。
- ユーザーが「インタビュー形式」「質問しながら」などを指定した場合、質問は一度に1つずつ行う。
- ユーザーが事象を共有したら、次の順で整理する。
  1. 要約（3行）
  2. 問題と根本原因
  3. 解決手順
  4. 検証方法
  5. ノート草案
- 事実と推測を分離し、未検証情報は「未検証」と明記する。

## ノート化ルール
- ノート作成時は `wiki-config/TEMPLATE.md` に従う。
- タグは `wiki-config/tags.md` の既存語彙を優先する。
- ノート本文は再現可能な手順を優先し、冗長な背景説明は短く保つ。
- 保存先は `notes/` 配下とする。
- 混同防止のため、メタデータに `knowledge_origin` と `evidence_level` を必ず記載する。
  - `knowledge_origin`: firsthand | heard | derived
  - `evidence_level`: verified | unverified | mixed
- `横断的知見` と `次の問い` セクションは任意項目として扱う（あれば望ましい）。

## 2層構成（raw層 / wiki層）
このリポジトリは2層で運用する。

| 層 | 場所 | 単位 | 更新 |
|---|---|---|---|
| raw | `notes/` | 1ノート1結論・日付付き | 追記のみ |
| wiki | `wiki/` | 1ページ1テーマ（複数ノートの統合）・日付なし | 上書き |

- **書くときはnotes、引くときはwiki。** 新しい事実はまず `notes/` にノート化し、後から該当wikiページへ統合する。
- **wikiからnotesへ逆流させない。** wikiの編集でnotesを書き換えない。
- ノート側とwiki側の記述が食い違ったら**ノートが正**。wikiを直す。
- wikiページの書式は `wiki-config/WIKI-TEMPLATE.md` に従う。
- wikiページには**出典ノートを必ず列挙する**。出典を辿れない段落は書かない。
- 伝聞・未検証（`knowledge_origin: heard` / `evidence_level: unverified`）由来の記述は、wiki上でも「（伝聞・未検証）」と明記し、firsthandの記述と混ぜない。
- ノート側で解決していない問いは、wikiで解決済みに見せない。「未確定・次の問い」へ落とす。
- 案件固有情報を含むページは `portable: no` とし、外部へ共有できる層へそのまま持ち出さない。

## ファイル作成時の動作
- ユーザーが「ノート化」「Wiki化」「整理して」などを依頼した場合、ノート草案を提案し、合意後にファイルへ反映する。
- 既存ノートの更新時は、重複を避けるため類似ノートの有無を確認する。
- ノートを追加したら、対応するwikiページの「出典ノート」に追記する。統合が追いつかない場合も出典行だけは足す（未統合であることが見える状態にする）。

## 破壊的操作の制限
以下の操作は必ずユーザーに確認を取り、明示的な承認を得てから実行する。
- ノートの削除（ファイルそのものを消す）
- `status` を `deprecated` に変更する
- 既存セクションの内容を大幅に書き換える・削除する
- `notes/index.md` や `notes/log.md` の既存エントリを削除する

追記・新規作成・軽微な誤字修正は確認不要。

## テキスト更新時の禁止事項
- run_in_terminalでの一括テキスト再保存を避ける（PowerShell Encoding未指定による文字化けリスク）
- 複数ファイルの同時編集は apply_patch を優先し、Encodingを明示する
- テキスト処理は可能な限りパッチ編集（apply_patch）で実施する
- やむを得ず run_in_terminal を使う場合は常に -Encoding UTF8 を明示し、変更後に git diff と文字化けチェックを必須実施する

## PowerShell ファイル操作の Encoding ルール
Windows環境で PowerShell 5.1 を使用する際、以下を必須化する。

- Get-Content・Set-Content を使う場合、常に `-Encoding UTF8` を明示する。
  ```powershell
  # ✗ 禁止（既定は CP932、文字化けリスク）
  Get-Content -Path "file.md"
  Set-Content -Path "file.md" -Value $content
  
  # ✓ 推奨
  Get-Content -Path "file.md" -Encoding UTF8
  Set-Content -Path "file.md" -Encoding UTF8 -Value $content
  ```
- ファイル読み込み後に日本語が含まれていないことを目視確認する。
- ファイル操作の前後で `git status` と `git diff` を確認し、予期しない文字化けがないことを確認する。

（このルールは Windows + PowerShell 5.1 環境での文字化け事故から作られたもの。Linux / macOS では不要。）

## 運用参照
- ノートテンプレ: `wiki-config/TEMPLATE.md`
- wikiページテンプレ: `wiki-config/WIKI-TEMPLATE.md`
- 体系化層の入口（検索時に最初に参照）: `wiki/README.md`
- タグ体系: `wiki-config/tags.md`
- 会話運用手順: `wiki-config/conversation-workflow.md`
- 会話プロンプト例: `wiki-config/chat-prompts.md`
- 週次運用ルール: `wiki-config/operations.md`
- ノート一覧（検索時に最初に参照）: `notes/index.md`
- 更新履歴: `notes/log.md`
