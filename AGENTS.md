# AGENTS.md

## Spec Kit と Specify について

**GitHub Spec Kit** は Spec-Driven Development (SDD) を実現するための包括的なツールキットです。SDD は「実装前に明確な仕様を作成する」ことを重視する開発手法であり、Spec Kit はテンプレート・スクリプト・ワークフローを通じて、開発チームが構造化されたプロセスでソフトウェアを構築できるよう支援します。

**Specify CLI** は Spec Kit フレームワークを用いたプロジェクトのセットアップを行うコマンドラインインターフェースです。SDD ワークフローを支えるために、必要なディレクトリ構成・テンプレート・AI エージェント連携を自動で整備します。

このツールキットは複数の AI コーディングアシスタントをサポートしており、チームは好みのツールを利用しつつ、統一されたプロジェクト構造と開発プラクティスを維持できます。

---

## 一般的な取り扱い

- Specify CLI の `__init__.py` を変更する場合は、`pyproject.toml` のバージョンを更新し、`CHANGELOG.md` にエントリを追加してください。

## 新しいエージェントのサポート追加

このセクションでは、Specify CLI に新しい AI エージェント/アシスタントを追加する方法を説明します。Spec-Driven Development ワークフローへ新しい AI ツールを統合する際のリファレンスとして利用してください。

### 概要

Specify はプロジェクト初期化時に各エージェント専用のコマンドファイルとディレクトリ構造を生成することで、複数の AI エージェントをサポートします。エージェントごとに以下のような独自のルールがあります。

- **コマンドファイル形式** (Markdown、TOML など)
- **ディレクトリ構造** (`.claude/commands/`, `.windsurf/workflows/` など)
- **コマンド呼び出しパターン** (スラッシュコマンド、CLI ツールなど)
- **引数の渡し方** (`$ARGUMENTS`, `{{args}}` など)

### 現在サポートしているエージェント

| エージェント | ディレクトリ | フォーマット | CLI ツール | 説明 |
|--------------|--------------|--------------|------------|------|
| **Claude Code** | `.claude/commands/` | Markdown | `claude` | Anthropic 製 Claude Code CLI |
| **Gemini CLI** | `.gemini/commands/` | TOML | `gemini` | Google 製 Gemini CLI |
| **GitHub Copilot** | `.github/prompts/` | Markdown | なし (IDE ベース) | VS Code 用 GitHub Copilot |
| **Cursor** | `.cursor/commands/` | Markdown | `cursor-agent` | Cursor CLI |
| **Qwen Code** | `.qwen/commands/` | TOML | `qwen` | Alibaba 製 Qwen Code CLI |
| **opencode** | `.opencode/command/` | Markdown | `opencode` | opencode CLI |
| **Codex CLI** | `.codex/commands/` | Markdown | `codex` | Codex CLI |
| **Windsurf** | `.windsurf/workflows/` | Markdown | なし (IDE ベース) | Windsurf IDE ワークフロー |
| **Kilo Code** | `.kilocode/rules/` | Markdown | なし (IDE ベース) | Kilo Code IDE |
| **Auggie CLI** | `.augment/rules/` | Markdown | `auggie` | Auggie CLI |
| **Roo Code** | `.roo/rules/` | Markdown | なし (IDE ベース) | Roo Code IDE |
| **CodeBuddy** | `.codebuddy/commands/` | Markdown | `codebuddy` | CodeBuddy |
| **Amazon Q Developer CLI** | `.amazonq/prompts/` | Markdown | `q` | Amazon Q Developer CLI |

### 手順ガイド

仮の新エージェントを例に、追加手順を順に説明します。

#### 1. AGENT_CONFIG の更新

**重要**: キーには省略名ではなく実際の CLI ツール名を使用してください。

新しいエージェントを `src/specify_cli/__init__.py` 内の `AGENT_CONFIG` 辞書に追加します。ここがすべてのエージェントメタデータの **唯一の信頼できる情報源** です。

```python
AGENT_CONFIG = {
    # ... 既存エージェント ...
    "new-agent-cli": {  # 実際にターミナルで入力する CLI 名を指定
        "name": "New Agent Display Name",
        "folder": ".newagent/",  # エージェント専用ファイルの配置ディレクトリ
        "install_url": "https://example.com/install",  # インストールドキュメントの URL (IDE ベースなら None)
        "requires_cli": True,  # CLI ツールが必須なら True、IDE ベースなら False
    },
}
```

**設計上の重要な原則**: 辞書キーはユーザーがインストールする実行ファイル名と一致させてください。例:
- ✅ CLI が `cursor-agent` の場合は `"cursor-agent"` を使用
- ❌ CLI が `cursor-agent` なのに `"cursor"` と短縮しない

こうすることで、コードベース全体に特別なマッピングを散在させる必要がなくなります。

**フィールドの説明**:
- `name`: ユーザーに表示するわかりやすい名前
- `folder`: エージェント専用ファイルを配置するディレクトリ (プロジェクトルートからの相対パス)
- `install_url`: インストール手順の URL (IDE ベースなら `None`)
- `requires_cli`: プロジェクト初期化時に CLI ツールチェックが必要かどうか

#### 2. CLI のヘルプテキストを更新

`init()` コマンドの `--ai` パラメータ説明文に新しいエージェントを追記します。

```python
ai_assistant: str = typer.Option(
    None,
    "--ai",
    help="AI assistant to use: claude, gemini, copilot, cursor-agent, qwen, opencode, codex, windsurf, kilocode, auggie, codebuddy, new-agent-cli, or q",
)
```

また、利用可能なエージェントを列挙しているドキュメンテーションストリング、サンプル、エラーメッセージも更新してください。

#### 3. README のドキュメント更新

`README.md` の **Supported AI Agents** セクションに新しいエージェントを追加します。

- テーブルへエージェント名とサポートレベル (Full/Partial) を追記
- 公式サイトのリンクを含める
- 実装上の注意点があれば記載
- テーブルの整形を崩さないように調整

#### 4. リリースパッケージ作成スクリプトの更新

`.github/workflows/scripts/create-release-packages.sh` を修正します。

##### ALL_AGENTS 配列に追加

```bash
ALL_AGENTS=(claude gemini copilot cursor-agent qwen opencode windsurf q)
```

##### ディレクトリ構成の case 文に追加

```bash
case $agent in
  # ... 既存のケース ...
  windsurf)
    mkdir -p "$base_dir/.windsurf/workflows"
    generate_commands windsurf md "\$ARGUMENTS" "$base_dir/.windsurf/workflows" "$script" ;;
esac
```

#### 4. GitHub リリーススクリプトの更新

`.github/workflows/scripts/create-github-release.sh` を変更し、新しいエージェントの成果物を追加します。

```bash
gh release create "$VERSION" \
  # ... 既存のパッケージ ...
  .genreleases/spec-kit-template-windsurf-sh-"$VERSION".zip \
  .genreleases/spec-kit-template-windsurf-ps-"$VERSION".zip \
  # 新しいエージェントのパッケージをここに追加
```

#### 5. エージェントコンテキスト更新スクリプトの修正

##### Bash スクリプト (`scripts/bash/update-agent-context.sh`)

ファイル変数の追加:

```bash
WINDSURF_FILE="$REPO_ROOT/.windsurf/rules/specify-rules.md"
```

case 文の更新:

```bash
case "$AGENT_TYPE" in
  # ... 既存ケース ...
  windsurf) update_agent_file "$WINDSURF_FILE" "Windsurf" ;;
  "")
    # ... 既存チェック ...
    [ -f "$WINDSURF_FILE" ] && update_agent_file "$WINDSURF_FILE" "Windsurf";
    # デフォルト作成条件の更新
    ;;
esac
```

##### PowerShell スクリプト (`scripts/powershell/update-agent-context.ps1`)

ファイル変数の追加:

```powershell
$windsurfFile = Join-Path $repoRoot '.windsurf/rules/specify-rules.md'
```

switch 文の更新:

```powershell
switch ($AgentType) {
    # ... 既存ケース ...
    'windsurf' { Update-AgentFile $windsurfFile 'Windsurf' }
    '' {
        foreach ($pair in @(
            # ... 既存のペア ...
            @{file=$windsurfFile; name='Windsurf'}
        )) {
            if (Test-Path $pair.file) { Update-AgentFile $pair.file $pair.name }
        }
        # デフォルト作成条件の更新
    }
}
```

#### 6. CLI ツールチェックの更新 (必要に応じて)

CLI ツールが必須のエージェントであれば、`check()` コマンドや初期化時のバリデーションにチェックを追加します。

```python
# check() コマンド内
tracker.add("windsurf", "Windsurf IDE (optional)")
windsurf_ok = check_tool_for_tracker("windsurf", "https://windsurf.com/", tracker)

# init バリデーション (CLI ツール必須の場合のみ)
elif selected_ai == "windsurf":
    if not check_tool("windsurf", "Install from: https://windsurf.com/"):
        console.print("[red]Error:[/red] Windsurf CLI is required for Windsurf projects")
        agent_tool_missing = True
```

**注意**: 現在は `AGENT_CONFIG` の `requires_cli` フィールドに基づいて自動的にチェックされます。`check()` や `init()` コマンドで追加のコード変更は不要で、AGENT_CONFIG をループして必要に応じたツールチェックが実行されます。

## 重要な設計判断

### 実際の CLI ツール名をキーに使用する

**極めて重要**: AGENT_CONFIG にエージェントを追加する際は、必ず実行ファイル名そのものを辞書キーに使ってください (省略名を使わない)。

**理由**:
- `check_tool()` は `shutil.which(tool)` で PATH を探索する
- キーが実際の CLI 名と一致しないと、コード全体に特別なマッピングが必要になる
- これは複雑さと保守コストを不必要に増やす

**例: Cursor の教訓**

❌ **誤った例** (専用マッピングが必要になる):

```python
AGENT_CONFIG = {
    "cursor": {  # 実際のツール名と一致しない短縮形
        "name": "Cursor",
        # ...
    }
}

# その結果、あらゆる場所で特別扱いが必要になる:
cli_tool = agent_key
if agent_key == "cursor":
    cli_tool = "cursor-agent"  # 実際のツール名へマッピング
```

✅ **正しい例** (マッピング不要):

```python
AGENT_CONFIG = {
    "cursor-agent": {  # 実行ファイル名と完全に一致
        "name": "Cursor",
        # ...
    }
}

# 特別なマッピングなしでそのまま利用できる
```

**このアプローチの利点**:
- コードベース全体に散在する特別扱いロジックを排除
- メンテナンス性が向上し、理解しやすくなる
- 新しいエージェント追加時のバグ発生リスクを軽減
- ツールチェックが追加設定なしで動作する

## エージェントの分類

### CLI ベースのエージェント

インストール済みのコマンドラインツールが必要です。
- **Claude Code**: `claude` CLI
- **Gemini CLI**: `gemini` CLI  
- **Cursor**: `cursor-agent` CLI
- **Qwen Code**: `qwen` CLI
- **opencode**: `opencode` CLI
- **CodeBuddy**: `codebuddy` CLI

### IDE ベースのエージェント

統合開発環境上で動作します。
- **GitHub Copilot**: VS Code などに組み込み
- **Windsurf**: Windsurf IDE に組み込み

## コマンドファイル形式

### Markdown 形式

使用エージェント: Claude、Cursor、opencode、Windsurf、Amazon Q Developer

```markdown
---
description: "Command description"
---

Command content with {SCRIPT} and $ARGUMENTS placeholders.
```

### TOML 形式

使用エージェント: Gemini、Qwen

```toml
description = "Command description"

prompt = """
Command content with {SCRIPT} and {{args}} placeholders.
"""
```

## ディレクトリ命名規則

- **CLI エージェント**: 通常は `.<エージェント名>/commands/`
- **IDE エージェント**: IDE 固有の構造に従う
  - Copilot: `.github/prompts/`
  - Cursor: `.cursor/commands/`
  - Windsurf: `.windsurf/workflows/`

## 引数のパターン

エージェントごとに利用するプレースホルダーが異なります。
- **Markdown/プロンプト形式**: `$ARGUMENTS`
- **TOML 形式**: `{{args}}`
- **スクリプトプレースホルダー**: `{SCRIPT}` (実際のスクリプトパスに置き換え)
- **エージェントプレースホルダー**: `__AGENT__` (エージェント名に置き換え)

## 新しいエージェント統合のテスト

1. **ビルドテスト**: パッケージ作成スクリプトをローカルで実行
2. **CLI テスト**: `specify init --ai <agent>` コマンドを実行
3. **ファイル生成確認**: 正しいディレクトリ構造とファイルが生成されているか検証
4. **コマンド検証**: 生成されたコマンドがエージェントで動作するか確認
5. **コンテキスト更新**: エージェントコンテキスト更新スクリプトをテスト

## よくある落とし穴

1. **省略形キーの使用**: AGENT_CONFIG のキーは必ず実行ファイル名にする (例: `"cursor-agent"`、`"cursor"` は不可)。これで特別なマッピングを回避できます。
2. **更新スクリプトの失念**: Bash と PowerShell の両方のスクリプトを更新すること。
3. **`requires_cli` の誤設定**: 実際に CLI ツールが必要な場合だけ `True`。IDE ベースなら `False`。
4. **引数形式の誤り**: エージェントに合ったプレースホルダーを使用 (Markdown は `$ARGUMENTS`, TOML は `{{args}}`)。
5. **ディレクトリ命名ミス**: エージェント固有の命名規則を厳守 (既存エージェントを参考に)。
6. **ヘルプテキストの不整合**: ユーザー向け文言 (ヘルプ、ドキュメント、エラーメッセージ) をすべて更新。

## 今後の検討事項

新しいエージェントを追加する際は次を意識してください。

- エージェント固有のコマンド/ワークフローパターンを考慮する
- Spec-Driven Development プロセスとの整合性を確保する
- 特別な要件や制限があれば明確に記録する
- 本書を最新情報に更新する
- AGENT_CONFIG 追加前に実際の CLI ツール名を必ず確認する

---

*新しいエージェントを追加した際は、本ドキュメントも最新化し、正確性と完全性を維持してください。*
