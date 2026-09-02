# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## リポジトリの目的

HTML の標準機能（`<dialog>`、フォームの標準バリデーション等）の挙動を実際に触って検証するための素振り用リポジトリ。ビルドステップやフレームワークはなく、ブラウザで生の HTML/CSS を開いて挙動を確認する。

## 開発コマンド

ツールは mise 経由で管理する（`mise.toml` 参照）。初回は `mise install` でツール（`pnpm`, `http-server`）を揃える。

- **開発サーバ起動**: `mise run dev` — `http-server -o` を実行し、既定ブラウザで自動的にトップ（`index.html`）を開く。
- **フォーマット**: prettier が devDependency に入っているので、必要に応じて `pnpm exec prettier --write <file>`。ローカル設定ファイルは無いのでデフォルト設定。

テストランナーは無い（HTML の挙動確認そのものが「テスト」なのでブラウザで目視確認する）。`package.json` の `test` スクリプトはプレースホルダで実行しない。

## アーキテクチャ

### 全体構成

- **`index.html`** はリンク集（目次）ページ。各検証ページへのエントリポイントであり、フォーム等の検証ロジックそのものは持たない。
- **`〇〇.html`**（例: `form.html`）が実際の検証ページ。1テーマ = 1 HTML ファイル。
- ページはすべて**リポジトリのルート直下にフラット配置**する。サブディレクトリでのカテゴリ分けはしない。
- CSS は**検証ページごとに独立**（`form.html` ↔ `form.css`、`dialog.html` ↔ `dialog.css` の1対1対応）。共有 CSS ファイル（旧 `style.css` 相当）は持たない。ページ間でスタイルの副作用が起きないようにするための意図的な設計。

### 新しい検証ページを追加する手順

1. `〇〇.html` を新規作成。`<link rel="stylesheet" href="〇〇.css">` を入れ、`<body>` 冒頭に `<a href="index.html">← 目次に戻る</a>` の導線を置く。
2. 必要なら `〇〇.css` を新規作成（スタイルが要らないページなら省略可）。
3. `index.html` の `<ul class="page-list">` に `<li><a href="〇〇.html">タイトル</a> — 何を検証しているかの一言</li>` を1行追加。

`index.html` のリンクには「何を検証しているか」を短く添える運用にしている（ページが増えたときの自己ドキュメント化のため）。

### 言語・文言

- 検証ページの本文・見出し・コメントは日本語。
- `<html lang="ja">` を必ず指定する。
