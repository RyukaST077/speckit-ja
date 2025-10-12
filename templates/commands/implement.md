---
description: tasks.md に定義されたタスクを順次実行し、実装計画を完了させます。
scripts:
  sh: scripts/bash/check-prerequisites.sh --json --require-tasks --include-tasks
  ps: scripts/powershell/check-prerequisites.ps1 -Json -RequireTasks -IncludeTasks
---

## ユーザー入力

```text
$ARGUMENTS
```

入力がある場合は必ず考慮した上で処理を進めてください。

## アウトライン

1. リポジトリルートで `{SCRIPT}` を実行し、FEATURE_DIR と AVAILABLE_DOCS を取得します。パスはすべて絶対パスに変換してください。"I'm Groot" のようなシングルクォートを含む引数は `'I'\''m Groot'` (または `"I'm Groot"`) としてエスケープします。

2. **チェックリストの状況を確認** (FEATURE_DIR/checklists/ が存在する場合):
   - checklists/ 内のファイルを走査
   - 各チェックリストについて以下を集計:
     * 総項目数: `- [ ]` / `- [X]` / `- [x]` の行
     * 完了項目数: `- [X]` / `- [x]`
     * 未完了項目数: `- [ ]`
   - 状態テーブルを作成:
     ```
     | Checklist | Total | Completed | Incomplete | Status |
     |-----------|-------|-----------|------------|--------|
     | ux.md     | 12    | 12        | 0          | ✓ PASS |
     | test.md   | 8     | 5         | 3          | ✗ FAIL |
     | security.md | 6   | 6         | 0          | ✓ PASS |
     ```
   - 総合ステータス:
     * **PASS**: 未完了項目が 0 のチェックリストのみ
     * **FAIL**: 未完了項目が 1 つでも存在
   
   - **未完了項目がある場合**:
     * テーブルを表示
     * **処理を停止** し、「チェックリストに未完了項目があります。実装を続行しますか？ (yes/no)」と確認
     * ユーザーの応答を待つ
     * `no`・`wait`・`stop` → 実行停止
     * `yes`・`proceed`・`continue` → ステップ 3 へ
   
   - **すべて完了している場合**:
     * テーブルで PASS を表示
     * 自動的にステップ 3 へ進む

3. **実装コンテキストを読み込み・分析**:
   - **必須**: tasks.md でタスク一覧と実行計画を確認
   - **必須**: plan.md で技術スタック、アーキテクチャ、ファイル構成を取得
   - **存在すれば参照**:
     * data-model.md: エンティティとリレーション
     * contracts/: API 仕様とテスト要件
     * research.md: 技術決定や制約
     * quickstart.md: 統合シナリオ

4. **プロジェクトセットアップの検証**:
   - 実際のプロジェクト状況に応じた ignore ファイルを作成/検証
   - Git リポジトリ判定:
     ```sh
     git rev-parse --git-dir 2>/dev/null
     ```
   - Dockerfile* の存在または plan.md に Docker 記載 → `.dockerignore`
   - `.eslintrc*` / `eslint.config.*` → `.eslintignore`
   - `.prettierrc*` → `.prettierignore`
   - `.npmrc` または `package.json` → `.npmignore` (公開する場合)
   - `*.tf` → `.terraformignore`
   - Helm チャートがあれば `.helmignore`
   
   **既存ファイルがある場合**: 必須パターンが含まれているか検証し、不足分のみ追記  
   **ファイルがない場合**: 検知した技術に応じた完全なパターンセットで新規作成
   
   **技術別の一般的パターン**:
   - **Node.js/JavaScript**: `node_modules/`, `dist/`, `build/`, `*.log`, `.env*`
   - **Python**: `__pycache__/`, `*.pyc`, `.venv/`, `venv/`, `dist/`, `*.egg-info/`
   - **Java**: `target/`, `*.class`, `*.jar`, `.gradle/`, `build/`
   - **C#/.NET**: `bin/`, `obj/`, `*.user`, `*.suo`, `packages/`
   - **Go**: `*.exe`, `*.test`, `vendor/`, `*.out`
   - **共通**: `.DS_Store`, `Thumbs.db`, `*.tmp`, `*.swp`, `.vscode/`, `.idea/`
   
   **ツール別パターン例**:
   - **Docker**: `node_modules/`, `.git/`, `Dockerfile*`, `.dockerignore`, `*.log*`, `.env*`, `coverage/`
   - **ESLint**: `node_modules/`, `dist/`, `build/`, `coverage/`, `*.min.js`
   - **Prettier**: `node_modules/`, `dist/`, `build/`, `coverage/`, `package-lock.json`, `yarn.lock`, `pnpm-lock.yaml`
   - **Terraform**: `.terraform/`, `*.tfstate*`, `*.tfvars`, `.terraform.lock.hcl`

5. tasks.md の構造を解析し、以下を抽出:
   - **フェーズ**: Setup, Tests, Core, Integration, Polish など
   - **依存関係**: 直列/並列実行ルール
   - **タスク詳細**: ID, 説明, ファイルパス, 並列マーカー [P]
   - **実行フロー**: 順序と依存条件

6. タスク計画に従って実装を進める:
   - **フェーズ単位**で順番に完了させる
   - **依存関係**を尊重し、直列タスクは順番通りに実行
   - [P] が付いたタスクは同時実行可
   - **TDD アプローチ**: テストタスクがある場合は実装前に実行/作成
   - **ファイル単位の調整**: 同一ファイルに影響するタスクは直列で処理
   - **チェックポイント**: 各フェーズ完了時に検証し、問題があれば早期に対処

7. 実装ルール:
   - **セットアップ先行**: プロジェクト構造、依存関係、設定を整備
   - **テスト先行**: コントラクト・エンティティ・統合シナリオのテストが要求される場合
   - **コア実装**: モデル、サービス、CLI、エンドポイントを順次実装
   - **統合作業**: DB 接続、ミドルウェア、ログ、外部サービス連携など
   - **仕上げ**: 単体テスト、パフォーマンス最適化、ドキュメント更新など

8. 進捗トラッキングとエラー処理:
   - タスク完了毎に進捗を報告
   - 直列タスクで失敗が発生した場合は処理を停止し理由を提示
   - 並列タスクでの失敗は成功タスクを継続しつつ失敗を報告
   - 明確なエラーメッセージを提供し、デバッグの指針を提示
   - 実装継続が困難な場合は次のステップを提案
   - **重要**: 完了したタスクは tasks.md 上で `[X]` に更新すること

9. 完了確認:
   - 必須タスクがすべて完了しているか検証
   - 実装結果が元の仕様と一致しているか確認
   - テストが成功し、要求されたカバレッジに達しているか検証
   - 実装が技術計画に従っているか確認
   - 完了報告として、実装内容の概要と完了状況を提示

注意: このコマンドは tasks.md に完全なタスク分解が存在することを前提としています。不足している場合は `/tasks` を実行して再生成するよう提案してください。
