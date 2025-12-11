# [PROJECT_NAME] 憲章
<!-- 例: Spec 憲章, TaskFlow 憲章など -->

## 基本原則

### [PRINCIPLE_1_NAME]
<!-- 例: I. ライブラリファースト -->
[PRINCIPLE_1_DESCRIPTION]
<!-- 例: すべての機能はスタンドアロンライブラリとして開始; ライブラリは自己完結型で、独立してテスト可能かつドキュメント化されている必要がある; 明確な目的が必要 - 組織用のみのライブラリは不可 -->

### [PRINCIPLE_2_NAME]
<!-- 例: II. CLI インターフェース -->
[PRINCIPLE_2_DESCRIPTION]
<!-- 例: すべてのライブラリは CLI を介して機能を公開; テキスト入出力プロトコル: stdin/args → stdout、エラー → stderr; JSON + 人間が読める形式をサポート -->

### [PRINCIPLE_3_NAME]
<!-- 例: III. テストファースト（譲れない原則） -->
[PRINCIPLE_3_DESCRIPTION]
<!-- 例: TDD 必須: テスト作成 → ユーザー承認 → テスト失敗 → 実装; Red-Green-Refactor サイクルを厳格に実施 -->

### [PRINCIPLE_4_NAME]
<!-- 例: IV. 統合テスト -->
[PRINCIPLE_4_DESCRIPTION]
<!-- 例: 統合テストが必要な領域: 新しいライブラリ契約テスト、契約変更、サービス間通信、共有スキーマ -->

### [PRINCIPLE_5_NAME]
<!-- 例: V. 可観測性, VI. バージョニングと破壊的変更, VII. シンプルさ -->
[PRINCIPLE_5_DESCRIPTION]
<!-- 例: テキスト I/O によりデバッグ可能性を確保; 構造化ロギングが必要; または: MAJOR.MINOR.BUILD 形式; または: シンプルに始める、YAGNI 原則 -->

## [SECTION_2_NAME]
<!-- 例: 追加の制約、セキュリティ要件、パフォーマンス基準など -->

[SECTION_2_CONTENT]
<!-- 例: 技術スタック要件、コンプライアンス基準、デプロイメントポリシーなど -->

## [SECTION_3_NAME]
<!-- 例: 開発ワークフロー、レビュープロセス、品質ゲートなど -->

[SECTION_3_CONTENT]
<!-- 例: コードレビュー要件、テストゲート、デプロイメント承認プロセスなど -->

## ガバナンス
<!-- 例: 憲章はすべての他のプラクティスに優先する; 修正には文書化、承認、移行計画が必要 -->

[GOVERNANCE_RULES]
<!-- 例: すべての PR/レビューはコンプライアンスを検証する必要がある; 複雑さは正当化される必要がある; ランタイム開発ガイダンスには [GUIDANCE_FILE] を使用 -->

**バージョン**: [CONSTITUTION_VERSION] | **批准日**: [RATIFICATION_DATE] | **最終修正日**: [LAST_AMENDED_DATE]
<!-- 例: バージョン: 2.1.1 | 批准日: 2025-06-13 | 最終修正日: 2025-07-16 -->