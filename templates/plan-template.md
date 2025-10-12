# 実装計画: [FEATURE]

**ブランチ**: `[###-feature-name]` | **日付**: [DATE] | **仕様**: [link]  
**入力**: `/specs/[###-feature-name]/spec.md` のフィーチャー仕様書

**注記**: このテンプレートは `/speckit.plan` コマンドによって自動で埋められます。実行フローについては `.specify/templates/commands/plan.md` を参照してください。

## サマリー

[フィーチャー仕様から抽出: 主要要件 + リサーチに基づく技術的アプローチ]

## 技術コンテキスト

<!--
  要対応: このセクションの内容をプロジェクトの技術詳細に置き換えてください。
  下記の構成は、反復プロセスを導くためのガイドです。
-->

**言語/バージョン**: [例: Python 3.11, Swift 5.9, Rust 1.75 または NEEDS CLARIFICATION]  
**主要依存関係**: [例: FastAPI, UIKit, LLVM または NEEDS CLARIFICATION]  
**ストレージ**: [該当する場合: PostgreSQL, CoreData, ファイルなど / N/A]  
**テスト**: [例: pytest, XCTest, cargo test または NEEDS CLARIFICATION]  
**対象プラットフォーム**: [例: Linux サーバー, iOS 15+, WASM または NEEDS CLARIFICATION]  
**プロジェクト種別**: [single/web/mobile など。ソース構成を決定]  
**パフォーマンス目標**: [ドメイン固有。例: 1000 req/s, 10k lines/sec, 60 fps または NEEDS CLARIFICATION]  
**制約**: [ドメイン固有。例: p95 <200ms, メモリ <100MB, オフライン対応など または NEEDS CLARIFICATION]  
**想定スケール/範囲**: [ドメイン固有。例: 1 万ユーザー, 100 万 LOC, 50 画面 または NEEDS CLARIFICATION]

## 憲章チェック

*ゲート: フェーズ 0 のリサーチ開始前に必ず合格すること。フェーズ 1 の設計後に再確認すること。*

[憲章ファイルに基づき設定されるゲート項目]

## プロジェクト構成

### ドキュメント (本フィーチャー)

```
specs/[###-feature]/
├── plan.md              # このファイル (/speckit.plan コマンド出力)
├── research.md          # フェーズ 0 出力 (/speckit.plan コマンド)
├── data-model.md        # フェーズ 1 出力 (/speckit.plan コマンド)
├── quickstart.md        # フェーズ 1 出力 (/speckit.plan コマンド)
├── contracts/           # フェーズ 1 出力 (/speckit.plan コマンド)
└── tasks.md             # フェーズ 2 出力 (/speckit.tasks コマンド。/speckit.plan では作成しない)
```

### ソースコード (リポジトリ直下)
<!--
  要対応: 以下のプレースホルダーを、このフィーチャーの具体的な構成に置き換えてください。
  未使用の選択肢は削除し、選択した構成は実際のパス (例: apps/admin, packages/something) で展開してください。
  提出する計画には「Option」表記を残さないこと。
-->

```
# 【未使用なら削除】選択肢 1: 単一プロジェクト (デフォルト)
src/
├── models/
├── services/
├── cli/
└── lib/

tests/
├── contract/
├── integration/
└── unit/

# 【未使用なら削除】選択肢 2: Web アプリケーション ("frontend" + "backend" を検出した場合)
backend/
├── src/
│   ├── models/
│   ├── services/
│   └── api/
└── tests/

frontend/
├── src/
│   ├── components/
│   ├── pages/
│   └── services/
└── tests/

# 【未使用なら削除】選択肢 3: モバイル + API ("iOS/Android" を検出した場合)
api/
└── [上記 backend と同様]

ios/ または android/
└── [プラットフォーム固有構成: フィーチャーモジュール, UI フロー, プラットフォームテスト]
```

**構成の決定**: [選択した構成を記載し、上記に記した実際のディレクトリを参照]

## 複雑性トラッキング

*憲章チェックで違反が発生し、正当化が必要な場合のみ記入すること*

| 違反内容 | 必要な理由 | 却下したより単純な代替案 |
|-----------|------------|-------------------------------------|
| [例: 4 つ目のプロジェクト] | [現在の必要性] | [3 プロジェクトでは不十分な理由] |
| [例: リポジトリパターン] | [特定の課題] | [直接 DB アクセスでは不十分な理由] |
