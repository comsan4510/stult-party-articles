# stult-party-articles

[Stult Party](https://stult-party.com) の SEO 記事の**正本リポジトリ**。
ここに Markdown を push すると、デプロイ時に静的 HTML へビルドされて
`https://stult-party.com/a/` で公開される。

- ビルダー: discom リポジトリの `backend/scripts/build_articles.py`
- 配信: nginx が静的配信（DB・バックエンド・同期は一切介在しない）
- この README 自体は記事として取り込まれない

## 記事の書き方

リポジトリ直下（またはサブディレクトリ）に `.md` を 1 記事 1 ファイルで置く。
**ファイル名がそのまま URL** になる: `server-guide.md` → `/a/server-guide/`
（ファイル名は英小文字・数字・ハイフン推奨）。

```markdown
---
title: 記事タイトル
description: 検索結果に表示される説明文（80〜120文字目安・省略可）
date: 2026-08-03
---

本文（Markdown）。最初の見出しは h2 (`##`) から使うと構成が綺麗。
```

- `title` を省略すると最初の `# 見出し`、それも無ければファイル名がタイトルになる
- `date` を省略するとビルド日になる（記事一覧は date の降順）
- 同名ファイルが複数ディレクトリにある場合は先勝ちで片方だけ取り込まれる

## 反映のタイミング

1. **自動**: この repo への push → `.github/workflows/notify-discom.yml` が
   discom に `repository_dispatch` を送り、テスト → ビルド → デプロイが走る。
   **要設定**: この repo の Settings → Secrets and variables → Actions に
   `DISCOM_DISPATCH_TOKEN`（comsan4510/discom への repo 権限を持つ PAT）を登録。
   未設定の間は自動通知をスキップする（workflow は失敗しない）。
2. **手動**: discom の Actions → CI/CD → Run workflow。
3. discom 本体への通常 push でも記事は毎回リビルドされる。

反映まで push から 6〜7 分程度（CI テスト + Docker ビルド込み）。
