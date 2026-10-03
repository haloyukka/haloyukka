# LIFE.OS

[← Engineer Profile](../profile.md)

- **Type:** Personal data archive / AI-assisted content pipeline
- **Status:** 🟢 Active
- **Repository:** [haloyukka/haloyukka](https://github.com/haloyukka/haloyukka)
- **Stack:** Markdown / YAML Front Matter / GitHub / Claude Code (Skills)

## Problem

ライブ・音楽・グルメ・日々の記録が散らばっていて、後から検索・集計しにくい。
一方で、最初からデータベースやアプリを作ると運用が重く、続かない。

## Idea

GitHub プロフィールリポジトリを「個人データのポータル」として設計する。

> **Readable by humans, usable by machines.**

- 人間が GitHub 上でそのまま読める Markdown を正とする
- メタデータは YAML Front Matter に持たせ、後から機械処理できるようにする
- 入力作業は AI に任せ、最終確認だけを人間が行う

## Architecture

```text
haloyukka/haloyukka
├─ README.md               # ポータル（各アーカイブへの入口）
├─ live-archive/
│  ├─ CLAUDE.md            # データ規約（正本）
│  ├─ templates/           # 公演ファイルのテンプレート
│  └─ setlists/YYYY/<event>/YYYY-MM-DD_<event>-<location>-<performance>.md
└─ .claude/skills/
   └─ add-setlist/         # AI 半自動生成のワークフロー
```

```text
イベント指定 → 情報源確認 → 曲情報・公式動画の調査 → Markdown 生成（verified: false）
            → 人間が確認 → verified: true
```

## Implementation

- **データモデル:** 1公演1ファイル。`date` / `event` / `location` / `venue` / `performance` / `attended` / `verified` を Front Matter で管理
- **品質管理:** 推測で値を埋めることを禁止し、不明な値は `Unknown` / `Not available` / `Unclassified` で明示。AI が生成した内容は必ず `verified: false` で始める
- **AI ワークフロー:** Claude Code のスキルで、調査 → 生成 → README 更新 → 完了条件のセルフチェックまでを手順化
- **リンク整合性:** Setlist から Track Details へのページ内リンクを、GitHub のアンカー生成規則（github-slugger）で検証

## Result

- セットリストの記録方法を、1公演1ファイルの規約として統一
- AI に「イベント名だけ」を渡せば、規約どおりの下書きが作られる運用を整備
- 情報源に接続できない状況でも、推測で埋めずに「未確認」と分かる形で記録できることを確認

## Lessons Learned

- AI にデータ入力を任せる場合は、「何を書くか」より「分からないときにどう書くか」を先に決めておく
- 最初から自動化しない。Markdown → Front Matter → 自動生成 → マスター化の順に、必要になった段階で拡張する
