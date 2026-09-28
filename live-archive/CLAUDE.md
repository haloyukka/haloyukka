# live-archive 規約

ライブ・イベントのセットリストを Markdown で記録するアーカイブ。

> **Readable by humans, usable by machines.**

新規公演の追加は `/add-setlist` スキル（`.claude/skills/add-setlist/`）を使う。
本ファイルはスキル・人間の双方が従う規約の正本。

## ディレクトリ

```text
live-archive/
├── README.md              # 全公演のインデックス（年 → イベント → 公演）
├── CLAUDE.md              # 本規約
├── templates/
│   └── setlist.md         # 公演ファイルのテンプレート
└── setlists/
    └── YYYY/
        └── <event-slug>/
            └── YYYY-MM-DD_<event-slug>-<location>-<performance>.md
```

- 1公演1ファイル。同日の昼公演・夜公演も別ファイル。
- ディレクトリ名・ファイル名は ASCII / lowercase / kebab-case（例: `magical-mirai`, `miku-expo`, `snow-miku`）。
- 日本語の正式名称はファイル内部でのみ使う。
- `2026/magical-mirai-2026.md` などの公演横断の比較まとめは本規約の対象外（そのまま残す）。

## ファイル構成（この順番）

1. YAML Front Matter
2. `# ライブタイトル`
3. `## Live Information`
4. `## Setlist`
5. `## Track Details`
6. `## Sources`

感想・レビュー用の Notes セクションは設けない。

## YAML Front Matter

| Key | 内容 | 例 |
| --- | --- | --- |
| `event` | イベント正式名称 | `"初音ミク「マジカルミライ 2026」"` |
| `date` | 公演日（`YYYY-MM-DD`、文字列） | `"2026-08-29"` |
| `location` | 開催地域（英語・先頭大文字） | `"Tokyo"`, `"Osaka"`, `"Hamamatsu"` |
| `venue` | 会場 | `"幕張メッセ 国際展示場 9ホール"` |
| `performance` | 公演区分 | `"Day"`, `"Night"`, `"Day1"`, `"Day2"`, `"Main"` |
| `attended` | 本人が参加したか | `true` / `false` |
| `verified` | 人間が内容を確認済みか | AI 生成直後は必ず `false` |
| `tags` | 検索・分類用タグ | `初音ミク`, `マジカルミライ`, `東京`, `2026` |

`verified: true` にする前に、人間がセットリスト・曲順・曲名・Artist / Producer・Vocal・YouTube URL・Main Set / Encore 区分・公演情報を確認する。

## Setlist

- 区分見出しは `### Main Set` / `### Encore`。一覧と Track Details で同じ区分を使う。
- 出典で Main Set / Encore の境界が確認できない場合は、推測せず全曲を `### Unclassified` に入れる。
- MC・幕間映像は記録しない。
- 曲番号は公演全体の通し番号（Encore でリセットしない）。
- 一覧の各曲は Track Details へのページ内リンクとし、YouTube へ直接リンクしない。

```markdown
1. [Tell Your World](#01-tell-your-world)
```

### アンカーの作り方（GitHub 準拠）

Track Details の見出し `### 01. 曲名` から GitHub が自動生成するアンカーに合わせる。

1. 英字を小文字にする
2. 文字・数字・`_`・`-`・スペース以外（`.` `,` `!` `(` `)` `。` `！` `・` など）を削除する
3. スペースを `-` に置換する（連続した `-` はそのまま残す）

| 見出し | アンカー |
| --- | --- |
| `01. HELLO, NEW WORLD!` | `#01-hello-new-world` |
| `18. 天樂 -双響-` | `#18-天樂--双響-` |
| `16. エル・タンゴ・エゴイスタ` | `#16-エルタンゴエゴイスタ` |
| `24. Over Flow(er)` | `#24-over-flower` |

## Track Details

```markdown
### 01. Tell Your World

- **Artist / Producer:** kz
- **Vocal:**
  - 初音ミク
- **YouTube:** [▶ Official](URL)
```

- 曲名は公式表記（英字・大文字小文字・記号・スペース・日本語）に合わせる。略称にしない。
- 作詞・作曲の個別フィールドは設けない。
- Vocal は単独でも必ずリスト形式。
- 曲が重複しても各ファイルに情報を書く（自己完結。将来 `data/songs.yml` へ分離予定）。

## YouTube

- 公式動画のみ: 公式 MV、公式音源、Producer 本人の公式動画、公式プロジェクト・公式チャンネルの動画。
- 無断転載・ファンアップロード・出典不明・非公式ミラー・再アップロードは使わない。
- テキストリンクのみ（サムネイル画像は使わない）: `[▶ Official](URL)`
- 公式動画が確認できない場合は `- **YouTube:** Not available`

## Sources

ファイル末尾に使用した情報源を列挙する。一次情報・公式情報を優先し、公式で確認できない情報に外部情報源を使った場合はそれも明記する。

## 不明な情報

確認できない情報は推測で埋めず、不明と分かる形で残す。

| 項目 | 不明時の表記 |
| --- | --- |
| Artist / Producer | `Unknown` |
| Vocal | リスト要素 `Unknown` |
| YouTube | `Not available` |
| Main Set / Encore | `### Unclassified` |

## 禁止事項

- YouTube: 存在しない URL の生成、非公式動画での補完、URL の推測
- 曲情報: 曲名の独自簡略化、Producer の推測、Vocal の推測
- セットリスト: 曲順の推測、Main Set / Encore 区分の推測、出典不明情報を確定情報として扱うこと

## README

公演を追加・更新したら `live-archive/README.md` の一覧も更新する。

- 年（`## 2026`）→ イベント（`### イベント正式名称`）→ 公演表の順
- 表の列: `Date | Location | Venue | Performance | Attended | Verified`
- Performance 列は公演ファイルへのリンク
- Attended: `✅` / `—`、Verified: `✅` / `⏳`
- 値は各ファイルの Front Matter と一致させる（将来 Front Matter から自動生成する想定）

## 完了条件

- [ ] 1公演1Markdownになっている
- [ ] ファイル命名規則に準拠している
- [ ] YAML Front Matterが存在する
- [ ] Live Informationが記載されている
- [ ] Main Set / Encoreが適切に分類されている
- [ ] 曲番号が通し番号になっている
- [ ] SetlistからTrack Detailsへ移動できる
- [ ] 正式な曲名が使用されている
- [ ] Artist / Producerが記載されている
- [ ] Vocalがリスト形式で記載されている
- [ ] YouTubeが公式動画のみになっている
- [ ] 公式動画がない場合 `Not available` になっている
- [ ] Sourcesが記載されている
- [ ] AI生成直後は `verified: false` になっている
- [ ] 人間による確認後に `verified: true` へ変更されている
- [ ] READMEから対象公演へアクセスできる
