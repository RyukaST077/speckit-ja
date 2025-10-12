---
description: 自然言語のフィーチャー記述から仕様書を作成・更新します。
scripts:
  sh: scripts/bash/create-new-feature.sh --json "{ARGS}"
  ps: scripts/powershell/create-new-feature.ps1 -Json "{ARGS}"
---

## ユーザー入力

```text
$ARGUMENTS
```

`/speckit.specify` コマンドの引数として入力されたテキストがフィーチャーの説明です。`{ARGS}` が文字どおり表示されていても、この会話内には必ずフィーチャー記述が存在するとみなしてください。空入力でない限り、ユーザーに再入力を求めないでください。

フィーチャー記述に基づき、以下を実行します:

1. `{SCRIPT}` をリポジトリルートで 1 度だけ実行し、JSON 出力から BRANCH_NAME と SPEC_FILE を取得します。すべてのパスは絶対パスです。  
   **重要**: このスクリプトは 1 回のみ実行してください。必要な情報はターミナルに表示された JSON から参照します。"I'm Groot" のようにシングルクォートを含む引数は `'I'\''m Groot'` もしくは `"I'm Groot"` としてエスケープします。
2. 仕様テンプレート `templates/spec-template.md` を読み込み、必要なセクションを把握します。

3. 次のフローに従います:

    1. 入力文からフィーチャー記述を解析  
       空の場合: `No feature description provided` としてエラー
    2. 主要概念を抽出  
       アクター、アクション、データ、制約を特定
    3. 不明点の扱い  
       - 文脈と業界標準から妥当な推測を行う  
       - 次の場合のみ `[NEEDS CLARIFICATION: 質問]` を付与 (最大 3 件):
         * フィーチャースコープやユーザー体験に大きな影響がある
         * 解釈が複数あり得て結果が変わる
         * 妥当なデフォルトが存在しない  
       - 優先度は影響度の高い順: スコープ > セキュリティ/プライバシー > UX > 技術詳細
    4. 「ユーザーシナリオ & テスト」セクションを記述  
       明確なユーザーフローが得られない場合: `Cannot determine user scenarios` としてエラー
    5. 機能要件を生成  
       すべての要件はテスト可能であること
    6. 成功指標を策定  
       計測可能かつ技術非依存の指標を用意  
       定量 (時間、性能、件数) と定性 (満足度、完遂率) を両方含める  
       実装詳細を含めず、検証可能な表現にする
    7. データを扱う場合は主要エンティティを定義
    8. 結果: 仕様がプランニング可能な状態になっていること

4. テンプレート構造に従って SPEC_FILE へ書き込みます。フィーチャー記述から得た具体的な内容でプレースホルダーを置き換え、セクション順序を維持します。

5. **仕様品質検証**: 初期仕様を書き込んだ後、以下の手順で品質を確認します:

   a. **仕様品質チェックリストの作成**: `FEATURE_DIR/checklists/requirements.md` に以下の項目を持つチェックリストを生成します (テンプレートの構造を使用):
   
      ```markdown
      # Specification Quality Checklist: [FEATURE NAME]
      
      **Purpose**: Validate specification completeness and quality before proceeding to planning
      **Created**: [DATE]
      **Feature**: [Link to spec.md]
      
      ## Content Quality
      
      - [ ] No implementation details (languages, frameworks, APIs)
      - [ ] Focused on user value and business needs
      - [ ] Written for non-technical stakeholders
      - [ ] All mandatory sections completed
      
      ## Requirement Completeness
      
      - [ ] No [NEEDS CLARIFICATION] markers remain
      - [ ] Requirements are testable and unambiguous
      - [ ] Success criteria are measurable
      - [ ] Success criteria are technology-agnostic (no implementation details)
      - [ ] All acceptance scenarios are defined
      - [ ] Edge cases are identified
      - [ ] Scope is clearly bounded
      - [ ] Dependencies and assumptions identified
      
      ## Feature Readiness
      
      - [ ] All functional requirements have clear acceptance criteria
      - [ ] User scenarios cover primary flows
      - [ ] Feature meets measurable outcomes defined in Success Criteria
      - [ ] No implementation details leak into specification
      
      ## Notes
      
      - Items marked incomplete require spec updates before `/speckit.clarify` or `/speckit.plan`
      ```
   
   b. **チェック実行**: 仕様を各項目に照らし合わせ、合否を判断し、問題があれば該当箇所を引用して記録します。
   
   c. **結果ハンドリング**:
      
      - **すべて合格**: チェックリストを完了済みとしてステップ 6 へ進む
      
      - **不合格項目がある ([NEEDS CLARIFICATION] を除く)**:
        1. 不合格項目と原因を列挙
        2. 仕様を更新して問題を解消
        3. 再度チェックを実行 (最大 3 回まで)
        4. それでも合格しない場合は、未完了項目と理由を報告し、改善案を提示
      
      - **[NEEDS CLARIFICATION] が残っている場合**:
        1. ユーザーに最大 3 件までの質問をまとめて提示 (明確なラベル Q1〜Q3)
        2. 各質問は次の要件を満たす:
           - オプション形式の場合: `Option | Candidate | Why It Matters` の表形式
           - 選択肢は最大 A〜E
           - ユーザーが選択する前に、各選択肢の意味を簡潔に説明
           - ユーザーの回答形式を明確化 (例: 「Q1: A, Q2: Custom - ...」)
        3. ユーザー回答を受けたら、仕様内の `[NEEDS CLARIFICATION]` 箇所を回答内容で置き換える
        4. 置き換え後に再度チェックリストを実行
        5. 表のフォーマット (パイプ位置、スペース、区切り線) を守る
        6. 質問は 3 件以内。同時に提示し回答を待つ
        7. すべての不明点が解消されたら次へ進む

   d. **チェックリストの更新**: 各検証サイクル後にチェックリストへ結果を反映します。

6. 完了報告として、ブランチ名、spec ファイルパス、チェックリスト結果、次フェーズ (`/speckit.clarify` または `/speckit.plan`) への準備状況を出力します。

**補足**: スクリプトは新しいブランチを作成してチェックアウトし、仕様ファイルを初期化します。

## ガイドライン (抜粋)

### クイックガイド

- **ユーザーのニーズ** と **ビジネス価値** に焦点を当てる
- 実装方法 (技術スタック、API、コード構造) を含めない
- 非技術ステークホルダー向けに記述
- チェックリストを仕様内に埋め込まない (別コマンドで生成)

### セクション要件

- **必須セクション**: すべて埋める
- **任意セクション**: 該当する場合のみ含める (不要なら項目ごと削除)

### AI が仕様を生成する際の指針

1. **妥当な推測を行う**: 文脈・業界標準・一般的パターンを使用
2. **仮定を記録**: Assumptions セクションに合理的なデフォルトを明示
3. **明確化要件を制限**: `[NEEDS CLARIFICATION]` は最大 3 件。以下に優先:
   - フィーチャースコープ/ユーザー体験に大きな影響
   - 解釈次第で結果が大きく変わる
   - 合理的なデフォルトが存在しない
4. **優先順位**: スコープ > セキュリティ/プライバシー > ユーザー体験 > 技術詳細
5. **テスターの視点**: テスト可能・曖昧さなしの要件のみ合格
6. **明確化が適切なケース (合理的デフォルトがない場合のみ)**:
   - フィーチャースコープと境界 (含める/除外するユースケース)
   - ユーザータイプと権限 (複数解釈が存在する場合)
   - セキュリティ/コンプライアンス要件 (法的/金銭的影響が大きい場合)

**合理的デフォルトの例 (質問不要)**:

- データ保持: 該当ドメインで一般的な慣行
- パフォーマンス目標: 標準的な Web/モバイルアプリの期待値
- エラーハンドリング: ユーザーフレンドリーなメッセージと適切なフォールバック
- 認証方式: Web アプリなら標準的なセッション方式または OAuth2
- 連携パターン: 指定がない場合は RESTful API

### 成功指標のガイドライン

成功指標は以下を満たす必要があります:

1. **測定可能**: 時間・割合・件数など具体的な指標
2. **技術非依存**: フレームワークや言語、DB、ツールを含めない
3. **ユーザー中心**: システム内部ではなくユーザー/ビジネス観点で記述
4. **検証可能**: 実装を知らなくても検証できる

**良い例**:

- 「ユーザーがチェックアウトを 3 分以内に完了できる」
- 「システムが 10,000 人の同時ユーザーを処理できる」
- 「検索結果の 95% が 1 秒以内に返る」
- 「主要タスクの達成率が 40% 向上する」

**悪い例** (実装に依存):

- 「API 応答時間が 200ms 未満」 (技術中心 → 「ユーザーは即座に結果を確認できる」などに変換)
- 「データベースが 1000 TPS を処理できる」 (ユーザー視点に置き換える)
- 「React コンポーネントが効率的にレンダリングされる」
- 「Redis キャッシュヒット率が 80% を超える」
