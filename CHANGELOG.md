# 変更履歴

<!-- markdownlint-disable MD024 -->

Specify CLI およびテンプレートに関する主な変更を記録します。

このファイルは [Keep a Changelog](https://keepachangelog.com/en/1.0.0/) の形式に基づき、[Semantic Versioning](https://semver.org/spec/v2.0.0.html) に従っています。

## [0.0.19] - 2025-10-10

### 追加

- CodeBuddy サポートを追加 ([`@lispking`](https://github.com/lispking) に感謝)。
- Specify CLI で Git 由来のエラーが確認できるようになりました。

### 変更

- `plan.md` 内の憲章ファイルパスを修正 ([`@lyzno1`](https://github.com/lyzno1) の指摘に感謝)。
- Gemini 用に生成される TOML ファイルのバックスラッシュエスケープを修正 ([`@hsin19`](https://github.com/hsin19) に感謝)。
- 実装コマンドが適切な ignore ファイルを追加するようになりました ([`@sigent-amazon`](https://github.com/sigent-amazon) に感謝)。

## [0.0.18] - 2025-10-06

### 追加

- `specify init .` を `--here` 相当として利用できるようにし、`.` を直感的なショートハンドとしてサポート。
- `/speckit.` 接頭辞で Spec Kit コマンドを容易に発見できるようにしました。
- プロンプトとテンプレートをリファクタリングし、不要なテスト生成を避けつつ能力を整理。
- タスクがユーザーストーリー単位で作成されるように改善 (テストと検証が容易に)。
- Visual Studio Code 向けのプロンプトショートカットと自動スクリプト実行を追加。

### 変更

- すべてのコマンドファイルを `speckit.` 接頭辞付きに変更 (例: `speckit.specify.md`, `speckit.plan.md`)。IDE や CLI のコマンドパレット/ファイル一覧で識別しやすくなりました。

## [0.0.17] - 2025-09-22

### 追加

- 既存仕様に対して最大 5 件の確認質問を行い、回答を仕様の Clarifications セクションに保存する `/clarify` コマンドテンプレートを追加。
- `/tasks` と `/implement` の間に挿入し、仕様・Clarifications・計画・タスク・憲章の不整合を分析する `/analyze` コマンドテンプレートを追加。
  - 備考: 憲章の原則は絶対であり、矛盾が見つかった場合は仕様側を修正する必要があります。

## [0.0.16] - 2025-09-22

### 追加

- `init` コマンドに `--force` フラグを追加。`--here` を空でないディレクトリで使用する際、確認をスキップしてファイルをマージ/上書きできます。

## [0.0.15] - 2025-09-21

### 追加

- Roo Code サポートを追加。

## [0.0.14] - 2025-09-21

### 変更

- エラーメッセージの表示を統一。

## [0.0.13] - 2025-09-21

### 追加

- Kilo Code サポートを追加 ([`@shahrukhkhan489`](https://github.com/shahrukhkhan489)、[#394](https://github.com/github/spec-kit/pull/394) に感謝)。
- Auggie CLI サポートを追加 ([`@hungthai1401`](https://github.com/hungthai1401)、[#137](https://github.com/github/spec-kit/pull/137) に感謝)。
- エージェントフォルダーに認証情報が保存される可能性があることを警告し、`.gitignore` への追加を推奨する注意書きを表示。

### 変更

- エージェントフォルダーを `.gitignore` に追加する必要があるかもしれない旨の警告を追加。
- `check` コマンドの出力を整理。

## [0.0.12] - 2025-09-21

### 変更

- OpenAI Codex ユーザー向けに追加の環境変数設定が必要である旨を説明 ([#417](https://github.com/github/spec-kit/issues/417) 参照)。

## [0.0.11] - 2025-09-20

### 追加

- Codex CLI サポートを追加 ([`@honjo-hiroaki-gtt`](https://github.com/honjo-hiroaki-gtt) による [#14](https://github.com/github/spec-kit/pull/14))。
- Codex に対応したコンテキスト更新ツール (Bash / PowerShell)。他エージェント同様に `AGENTS.md` を更新できるようになりました。

## [0.0.10] - 2025-09-20

### 修正

- GitHub トークンが空の場合でもリクエストへ付与されてしまう [#378](https://github.com/github/spec-kit/issues/378) を解消。

## [0.0.9] - 2025-09-19

### 変更

- エージェント選択 UI を改善し、キーをシアン、正式名称をグレーで表示。

## [0.0.8] - 2025-09-19

### 追加

- Windsurf IDE サポートを追加 ([`@raedkit`](https://github.com/raedkit) による [#151](https://github.com/github/spec-kit/pull/151))。
- 企業環境やレート制限対策として GitHub トークンによる API リクエストをサポート ([`@zryfish`](https://github.com/@zryfish) による [#243](https://github.com/github/spec-kit/pull/243))。

### 変更

- README に Windsurf の例と GitHub トークンの利用方法を追記。
- リリースワークフローに Windsurf テンプレートを含めるよう改善。

## [0.0.7] - 2025-09-18

### 変更

- CLI のコマンド説明を更新。
- 汎用情報のみの際にエージェント固有情報を表示しないようコードを整理。

## [0.0.6] - 2025-09-17

### 追加

- opencode を AI アシスタントオプションとして追加。

## [0.0.5] - 2025-09-17

### 追加

- Qwen Code を AI アシスタントオプションとして追加。

## [0.0.4] - 2025-09-14

### 追加

- 企業ネットワーク向けに SOCKS プロキシをサポート (`httpx[socks]` 依存関係を追加)。

### 修正

なし

### 変更

なし
