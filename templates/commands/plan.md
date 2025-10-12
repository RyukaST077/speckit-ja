---
description: プランテンプレートを用いて設計成果物を生成する実装計画ワークフローを実行します。
scripts:
  sh: scripts/bash/setup-plan.sh --json
  ps: scripts/powershell/setup-plan.ps1 -Json
agent_scripts:
  sh: scripts/bash/update-agent-context.sh __AGENT__
  ps: scripts/powershell/update-agent-context.ps1 -AgentType __AGENT__
---

## ユーザー入力

```text
$ARGUMENTS
```

入力がある場合は必ず考慮した上で処理を進めてください。

## アウトライン

1. **セットアップ**: リポジトリルートで `{SCRIPT}` を実行し、JSON から FEATURE_SPEC、IMPL_PLAN、SPECS_DIR、BRANCH を取得します。"I'm Groot" などシングルクォートを含む引数は `'I'\''m Groot'` または `"I'm Groot"` とエスケープしてください。

2. **コンテキストの読み込み**: FEATURE_SPEC と `/memory/constitution.md` を読み込み、コピー済みの IMPL_PLAN テンプレートを取得します。

3. **プランワークフローの実行**: IMPL_PLAN テンプレートの構成に従って以下を実施します:
   - Technical Context を埋める (不明点は "NEEDS CLARIFICATION" と記載)
   - 憲章に基づき Constitution Check セクションを埋める
   - ゲートを評価 (違反が正当化されない場合はエラー)
   - フェーズ 0: research.md を生成し、すべての NEEDS CLARIFICATION を解消
   - フェーズ 1: data-model.md、contracts/、quickstart.md を生成
   - フェーズ 1: エージェントスクリプトを実行してコンテキストを更新
   - フェーズ 1 完了後に再度 Constitution Check を評価

4. **停止して報告**: フェーズ 2 (tasks) 手前で終了し、ブランチ名・IMPL_PLAN パス・生成成果物を報告します。

## フェーズ

### フェーズ 0: アウトラインとリサーチ

1. Technical Context で抽出した未知項目:
   - 各 NEEDS CLARIFICATION をリサーチタスクへ変換
   - 各依存関係をベストプラクティスタスクへ変換
   - 各統合ポイントをパターンタスクへ変換

2. リサーチエージェントを生成し実行:
   ```
   各未知事項について:
     Task: "Research {unknown} for {feature context}"
   各技術選定について:
     Task: "Find best practices for {tech} in {domain}"
   ```

3. `research.md` に結果を統合:
   - Decision: [選択肢]
   - Rationale: [理由]
   - Alternatives considered: [検討した代替案]

**出力**: NEEDS CLARIFICATION が解消された `research.md`

### フェーズ 1: 設計とコントラクト

**前提条件**: `research.md` の完了

1. feature spec からエンティティを抽出し `data-model.md` を生成:
   - エンティティ名、フィールド、リレーション
   - 要求されるバリデーションルール
   - 状態遷移があれば含める

2. 機能要件から API コントラクトを生成:
   - 各ユーザーアクション → エンドポイント
   - REST/GraphQL の標準パターンを利用
   - `/contracts/` に OpenAPI/GraphQL スキーマを出力

3. **エージェントコンテキストの更新**:
   - `{AGENT_SCRIPT}` を実行
   - 使用中の AI エージェントを判定
   - 現在のプランで新たに使用する技術をエージェント用ファイルに反映
   - 手動追加領域はマーカー内で保持

**出力**: `data-model.md`、`/contracts/*`、`quickstart.md`、エージェント専用ファイル

## 主要ルール

- すべてのパスは絶対パスで扱う
- ゲート違反や未解決の明確化が残っている場合はエラーとして停止
