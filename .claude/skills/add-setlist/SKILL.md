---
name: add-setlist
description: live-archive にライブ・イベントの公演セットリスト Markdown を追加・補完する。「マジカルミライ2026 東京 夜公演」のようなイベント指定、またはユーザーが貼ったセットリストから、公式情報を調査して規約どおりの公演ファイルを生成し README の一覧を更新する。既存公演ファイルの Unknown / Not available の補完にも使う。
---

# add-setlist

規約の正本は `live-archive/CLAUDE.md`。作業前に必ず読み、そこに従う。
テンプレートは `live-archive/templates/setlist.md`。

## 入力パターン

- **A. イベント情報のみ**（例: `マジカルミライ2026 東京 夜公演`）: セットリストを Web で調査する。
- **B. セットリストをユーザーが提供**（例: `1. 曲名A 2. 曲名B …`）: 曲順はユーザー提供を正とし、不足情報を調査・補完する。
- **C. 既存ファイルの補完**: `Unknown` / `Not available` / `### Unclassified` の箇所だけを調査して埋める。確認できた箇所以外は変更しない。

公演を特定できない場合（日付・昼夜・開催地が曖昧など）は、生成前にユーザーへ確認する。

## フロー

1. **公演を特定する**: Event（正式名称）/ Date / Location / Venue / Performance
2. **セットリストを検索する**（パターン A）
3. **情報源を確認する**: 公式サイト・公式アフターレポート・公式 SNS を優先し、使った URL をすべて控える
4. **曲順と Main Set / Encore 区分を確認する**: 境界を出典で確認できなければ `### Unclassified`
5. **正式曲名を確認する**: 公式表記どおり。略称にしない
6. **Artist / Producer を確認する**: 確認できなければ `Unknown`
7. **Vocal を確認する**: 確認できなければ `Unknown`。「6人」「全員」などの集合表記は、出典に個別名がなければ展開しない
8. **公式 YouTube を検索する**: 公式チャンネル・Producer 本人のチャンネルであることを確認し、実在する URL のみ記載する。確認できなければ `Not available`
9. **Markdown を生成する**: `live-archive/setlists/YYYY/<event-slug>/YYYY-MM-DD_<event-slug>-<location>-<performance>.md`
   - `verified: false`
   - `attended` はユーザーに確認する（不明なら `false`）
   - Setlist のアンカーは CLAUDE.md の「アンカーの作り方」に従う
10. **README を更新する**: `live-archive/README.md` の年 → イベント → 公演表に行を追加する
11. **自己チェック**: CLAUDE.md の「完了条件」を 1 項目ずつ確認し、満たせない項目（不明情報など）をユーザーに報告する

## 厳守

- 推測で埋めない（曲順・区分・曲名・Producer・Vocal・YouTube URL すべて）
- 非公式動画で補完しない。URL を組み立てたり推測したりしない
- `verified: true` にするのは人間だけ。AI は変更しない
- 感想・Notes セクションを追加しない

## 完了報告

ユーザーには次の内容を返す。

- 作成・更新したファイル
- 使用した情報源
- `Unknown` / `Not available` / `Unclassified` として残した項目と、その理由
- 人間が確認すべき項目（`verified: true` にする前のチェック対象）
