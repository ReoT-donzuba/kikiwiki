# タイトル

> このページで言える結論を1〜2文で。読者はここだけ読んで判断できること。

## メタデータ
- layer: wiki
- updated_at: YYYY-MM-DD
- status: draft | stable | deprecated
- confidence: firsthand | mixed | heard-only
- portable: yes | no
- source_notes: N件

## （本文セクション）
テーマに応じて自由に構成する。ノートの時系列構造（背景→問題→試行ログ）は引き継がない。
読者が「次案件でどう使うか」を判断できる順序に並べ替える。

事実と伝聞・推測は必ず区別する。伝聞や未検証の内容には `（伝聞・未検証）` を付ける。

## 適用条件
- 前提条件:
- 副作用:
- 切り戻し:

## 未確定・次の問い
- ?

## 出典ノート
| ノート | 由来 | 確度 |
|---|---|---|
| [YYYYMMDD-xxx.md](../../notes/YYYYMMDD-xxx.md) | firsthand / heard / derived | verified / unverified / mixed |

---

## wikiページの書き方ルール

- **1ページ1テーマ**。ノートは「1ノート1結論」、wikiは「1ページ1テーマ（複数ノートの統合）」。
- **日付をファイル名に入れない**。wikiは上書き更新する正本。履歴はGitが持つ。
- **notes/ を書き換えない**。wikiはnotesから作るが、逆流させない。raw は追記のみ。
- **出典ノートを必ず列挙する**。どのノートから来た記述か辿れない段落は書かない。
- **`confidence` はページ内で最も弱い根拠に合わせる**。firsthandと伝聞が混ざれば `mixed`。
- **`portable: no` は案件固有情報を含むページ**。外部へ共有する際にこのページを経由させない。
- ノート側で解決していない問いは、解決したことにせず「未確定」に落とす。
