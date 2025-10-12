<div align="center">
    <img src="./media/logo_small.webp"/>
    <h1>🌱 Spec Kit</h1>
    <h3><em>高品質なソフトウェアをより速く。</em></h3>
</div>

<p align="center">
    <strong>Spec-Driven Development を活用し、差別化されないコードではなくプロダクト体験に集中するための取り組みです。</strong>
</p>

<p align="center">
    <a href="https://github.com/github/spec-kit/actions/workflows/release.yml"><img src="https://github.com/github/spec-kit/actions/workflows/release.yml/badge.svg" alt="Release"/></a>
    <a href="https://github.com/github/spec-kit/stargazers"><img src="https://img.shields.io/github/stars/github/spec-kit?style=social" alt="GitHub stars"/></a>
    <a href="https://github.com/github/spec-kit/blob/main/LICENSE"><img src="https://img.shields.io/github/license/github/spec-kit" alt="License"/></a>
    <a href="https://github.github.io/spec-kit/"><img src="https://img.shields.io/badge/docs-GitHub_Pages-blue" alt="Documentation"/></a>
</p>

---

## 目次

- [🤔 Spec-Driven Development とは?](#-spec-driven-development-とは)
- [⚡ はじめ方](#-はじめ方)
- [📽️ ビデオ概要](#️-ビデオ概要)
- [🤖 対応する AI エージェント](#-対応する-ai-エージェント)
- [🔧 Specify CLI リファレンス](#-specify-cli-リファレンス)
- [📚 中核となる思想](#-中核となる思想)
- [🌟 開発フェーズ](#-開発フェーズ)
- [🎯 実験的なゴール](#-実験的なゴール)
- [🔧 利用前提条件](#-利用前提条件)
- [📖 さらに学ぶ](#-さらに学ぶ)
- [📋 詳細なプロセス](#-詳細なプロセス)
- [🔍 トラブルシューティング](#-トラブルシューティング)
- [👥 メンテナー](#-メンテナー)
- [💬 サポート](#-サポート)
- [🙏 謝辞](#-謝辞)
- [📄 ライセンス](#-ライセンス)

## 🤔 Spec-Driven Development とは?

Spec-Driven Development (SDD) は、従来のソフトウェア開発の常識を覆します。長年、コードが主役であり、仕様は一時的な足場に過ぎませんでした。SDD ではこの力関係が逆転します。**仕様そのものが実行可能になり、コードは仕様から生成される成果物** です。つまり仕様は単なる指示書ではなく、動作する実装を直接生み出すプログラムのような存在になります。

## ⚡ はじめ方

### 1. Specify のインストール

お好みの方法を選んでください。

#### オプション 1: 永続的なインストール (推奨)

一度インストールすれば、どこでも利用できます。

```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

その後は直接コマンドを実行できます。

```bash
specify init <PROJECT_NAME>
specify check
```

アップグレードする場合:

```bash
uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git
```

#### オプション 2: 1 回限りの利用

インストールせずに直接実行します。

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME>
```

**永続的にインストールする利点**

- コマンドが PATH に常駐する
- シェルエイリアスを作る必要がない
- `uv tool list` / `uv tool upgrade` / `uv tool uninstall` で管理しやすい
- シェル設定を汚さない

### 2. プロジェクト憲章の策定

`/speckit.constitution` コマンドを使って、コード品質・テスト基準・ユーザー体験・性能要件など、プロジェクトを導く原則を定めます。

```bash
/speckit.constitution コード品質・テスト基準・ユーザー体験の一貫性・性能要件を重視した原則を作成してください
```

### 3. 仕様の作成

`/speckit.specify` コマンドで「作りたいもの」を記述します。技術要素ではなく **何を / なぜ** を中心に書きましょう。

```bash
/speckit.specify 写真を日付ごとにアルバムへ整理できるアプリを作りたい。アルバムはメインページでドラッグ&ドロップで並べ替えられる。ネストされたアルバムは存在しない。各アルバム内では写真をタイル状にプレビューする。
```

### 4. 技術的な実装計画の作成

`/speckit.plan` コマンドで技術スタックやアーキテクチャを指定します。

```bash
/speckit.plan Vite を使い、可能な限りライブラリを最小限にする。HTML/CSS/JavaScript はバニラで実装し、画像は外部にアップロードしない。メタデータはローカルの SQLite に保存する。
```

### 5. タスク分解

`/speckit.tasks` を実行すると、実装計画から実行可能なタスクリストが生成されます。

```bash
/speckit.tasks
```

### 6. 実装の実行

`/speckit.implement` を使って、計画通りにタスクを実行しフィーチャーを構築します。

```bash
/speckit.implement
```

より詳しい手順は [詳細ガイド](./spec-driven.md) を参照してください。

## 📽️ ビデオ概要

Spec-Driven Development と Spec Kit の全体像を短時間で把握したい場合は、以下のビデオを参照してください。

> **準備中**: 公式サイトのビデオセクションで順次公開予定です。

## 🤖 対応する AI エージェント

Spec Kit は複数の AI コーディングアシスタントに対応しており、初期化時に各エージェント専用のディレクトリ・コマンドテンプレートを生成します。サポート状況の詳細は [AGENTS.md](./AGENTS.md) を参照してください。

主な対応エージェント:

- Claude Code
- Gemini CLI
- Cursor
- Qwen Code
- opencode
- Codex CLI
- Windsurf
- Kilo Code
- Auggie CLI
- Roo Code
- CodeBuddy
- Amazon Q Developer CLI

## 🔧 Specify CLI リファレンス

Specify CLI は SDD ワークフロー全体を支えるコマンド群を提供します。主なコマンド:

| コマンド | 役割 |
|----------|------|
| `/speckit.constitution` | プロジェクト憲章の策定・更新 |
| `/speckit.specify` | 仕様 (spec.md) の作成 |
| `/speckit.plan` | 実装計画 (plan.md) と関連ドキュメントの生成 |
| `/speckit.tasks` | 実装タスクの生成 |
| `/speckit.analyze` | 成果物横断の品質/整合性レポート |
| `/speckit.clarify` | 仕様の不足点に対する質問と記録 |
| `/speckit.implement` | タスクの実行と実装 |

CLI の詳細は `.specify/templates/commands/` 内の各コマンドテンプレートや、[docs](https://github.github.io/spec-kit/) を参照してください。

## 📚 中核となる思想

SDD は以下の思想に基づいています。

1. **仕様が唯一の真実**: すべての成果物 (実装計画・コード・テスト・ドキュメント) は仕様から生まれる。
2. **憲章による統治**: プロジェクト固有の原則を明文化し、すべての意思決定と実装がそれに従う。
3. **テンプレートによる構造化**: あらゆる成果物にテンプレートを提供し、曖昧さを排除する。
4. **AI の活用**: 人間が意図と価値を表現し、AI が機械的な変換を担う。
5. **継続的な品質強化**: チェックリスト・分析・ clarify コマンドで仕様や計画を継続的に改善する。

## 🌟 開発フェーズ

SDD の標準フェーズ:

1. **憲章策定**: プロジェクトの原則を定義
2. **仕様作成**: ユーザー価値・要件・成功指標を明記
3. **リサーチ**: 不明点を調査し仕様を明確化
4. **実装計画**: アーキテクチャ・技術スタック・データモデルを決定
5. **タスク分解**: 実装計画を実行可能なタスクに落とし込む
6. **実装**: タスクを順に実行し、テスト駆動でコードを生成
7. **レビューと改善**: 仕様・計画・実装を定期的に見直し、リファインする

## 🎯 実験的なゴール

Spec Kit は次のようなゴールを念頭に設計されています。

- 仕様と実装の同期を常時保つ
- チーム全体で共通の成果物を参照し、意思決定を透明化する
- AI の助けを借りて反復サイクルを短縮する
- テスト駆動・契約テストを徹底し、現実的な動作保証を得る
- 無駄な抽象化を防ぎ、保守性の高いシンプルな構造を保つ

## 🔧 利用前提条件

Spec Kit を利用する際は以下の環境を整備してください。

- Git / GitHub CLI (任意)
- Python 3.11 以降 (CLI が依存)
- `uv` (推奨パッケージマネージャー)
- 対象エージェントに必要な CLI やトークン
- AI エージェントが扱う技術スタックの基礎知識

## 📖 さらに学ぶ

- [spec-driven.md](./spec-driven.md): SDD の詳細なガイド
- [AGENTS.md](./AGENTS.md): サポートしている AI エージェントと統合手順
- [memory/constitution.md](./memory/constitution.md): プロジェクトの憲章テンプレート
- [docs](https://github.github.io/spec-kit/): 公式ドキュメントサイト

## 📋 詳細なプロセス

SDD を用いた典型的なフローを以下にまとめます。

1. `/speckit.constitution` でプロジェクトの原則を確立
2. `/speckit.specify` で仕様を作成
3. `/speckit.clarify` で仕様の疑問点を洗い出し、記録
4. `/speckit.plan` で実装計画を生成 (research/data-model/contracts などを含む)
5. `/speckit.tasks` でフィーチャーを実行可能なタスクに分解
6. `/speckit.analyze` で仕様・計画・タスク間の整合性を検証
7. `/speckit.implement` でタスクを実行し、TDD でコードを生成
8. 完了後は仕様と計画を更新し、次の反復へ進む

各ステップの詳細な例や推奨プロンプトは [docs](https://github.github.io/spec-kit/) や [spec-driven.md](./spec-driven.md) の「Detailed Process」セクションを参照してください。

## 🔍 トラブルシューティング

### Linux で Git Credential Manager を利用する

Git 認証で問題が発生した場合は、以下のスクリプトで Git Credential Manager を導入できます。

```bash
#!/usr/bin/env bash
set -e
echo "Downloading Git Credential Manager v2.6.1..."
wget https://github.com/git-ecosystem/git-credential-manager/releases/download/v2.6.1/gcm-linux_amd64.2.6.1.deb
echo "Installing Git Credential Manager..."
sudo dpkg -i gcm-linux_amd64.2.6.1.deb
echo "Configuring Git to use GCM..."
git config --global credential.helper manager
echo "Cleaning up..."
rm gcm-linux_amd64.2.6.1.deb
```

その他のトラブルシューティング手順は [support](./SUPPORT.md) や [spec-driven.md](./spec-driven.md) にまとまっています。

## 👥 メンテナー

- Den Delimarsky ([@localden](https://github.com/localden))
- John Lam ([@jflam](https://github.com/jflam))

## 💬 サポート

サポートが必要な場合は [GitHub Issue](https://github.com/github/spec-kit/issues/new) を作成してください。バグ報告、機能要望、SDD の使い方についての質問を歓迎します。

## 🙏 謝辞

本プロジェクトは [John Lam](https://github.com/jflam) の研究・実践から大きな影響を受けています。

## 📄 ライセンス

本プロジェクトは MIT ライセンスで公開されています。詳細は [LICENSE](./LICENSE) を参照してください。
