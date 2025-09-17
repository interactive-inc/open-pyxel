---
applyTo: "**"
---

# Documentation Structure Rules

文書作成と更新時は明確な構造とセクション名を使用する。セクション名は英語で統一し、内容は日本語で記述する。必須セクションは指定された順序で含め、オプションセクションは必要に応じて追加する。

## Tools

### ファイルのID

以下のツールでファイルのIDを取得します。

- `docs-list-files` - repositories, requirements, terms, issues, notes
- `docs-list-products` - 全製品のIDと概要情報
- `docs-list-product-files` - 特定製品のファイルのID（features, routes, entities, notes）
- `docs-list-repository-issues` - 特定リポジトリのIssueのIDを取得

読み書き操作前に必ずリストして正しいIDを取得する。

### docs-write-product-route

ルートパス → pageIDの変換規則:

- `/` → `home.md`
- `/about` → `about.md`  
- `/a/b` → `a.b.md`
- `/users/profile` → `users.profile.md`

### docs-create-product

作成される構造:

```
products/
  [productId]/
    index.md          # 製品概要
    features/
      index.md        # 機能概要
    routes/
      index.md        # ルート概要
```

### docs-delete-product

削除される内容:

- 製品概要（index.md）
- 全機能と機能概要
- 全ルートとルート概要

## ファイルのマークダウン

重要:セクション名は固定で変更不可。以下の英語名を正確に使用する
- 必須セクションは指定順序で含める
- オプションセクション（"Optional"マーク付き）は省略可能だが使用時は正確な名前を使用
- セクション名の翻訳や変更は禁止
- テンプレートで指定されていないセクションの追加は禁止
- 言語規則:セクションヘッダーは英語、内容は日本語で記述
  - 例:`## Features`（ヘッダー英語） → 機能説明は日本語で記述

### Project Overview Files

プロジェクト全体の概要を記述し、提供価値、製品一覧、ターゲットユーザを整理した文書

#### 形式

```markdown
# [Project Name]

[One-line description of what this project offers to users]

## What We Provide

[Brief overview of the services and value this project delivers]

## Products

### [Product A Name]
[Brief description of what this product does and who uses it]

## Target Users

[Who this project serves and their primary needs]
- User Type A: [Their needs]
- User Type B: [Their needs]

## Core Value Proposition

[What makes this project unique and valuable]

## Context (Optional)

[Important background or decisions that shaped the project]
```

### Products Overview Files  

製品群の関係性や依存関係、データフローを整理した概要文書

#### 形式

```markdown
# Products Overview

[Brief description of all products in the system]

## Product Relationships

[How products interact and depend on each other]
- Product A → Product B relationship
- Shared components or services
- Data flow between products

## Architecture Context (Optional)

[High-level system architecture if relevant]
```

### Features Files

個別機能の手順や動作を番号付きリストで記述した機能仕様書

#### ファイル命名規約

`view-*`, `list-*`, `create-*`, `delete-*`, `add-*`, `remove-*`, `update-*`, `show-*`, `search-*`

#### 形式

```markdown
# [Feature Name (Action XXX)]

[Feature purpose and overview in 1-2 sentences]

## Context (Optional)

[Why this feature was designed this way]

## Note A (Optional)

[Additional information as needed]
```

### Routes Files

各ページやURLの目的と基本的な機能を記述したルート仕様書。詳細なUI仕様ではなく、何のためのページかが分かる最低限の情報を記録する。

#### ファイル命名規約

ルートパス → pageId: `/` → home.md, `/about` → about.md, `/a/b` → a.b.md

#### 形式

```markdown
# [Page Name]

[Page purpose and overview in 1-2 sentences]

## Context (Optional)

[Background on how this decision/implementation was reached]

## Note A (Optional)

[Additional information as needed]
```

### Repositories Files

リポジトリの責任範囲、依存関係、アーキテクチャ決定を記述した文書

#### ファイル命名規約

リポジトリ名を使用 `frontend-app.md`, `backend-api.md`

#### 形式

```markdown
# [Repository Name]

[Brief description of what this repository contains and its business purpose]

## Responsibility

[What this repository is responsible for in the system]
- Core functionality it provides
- Business domains it covers
- Services it exposes

## Dependencies

[Critical dependencies on other repositories and services]
- Which repositories it depends on
- External services it consumes
- Data sources it requires

## Architecture Decisions

[Key architectural patterns and decisions]
- Major design patterns used
- Important technical choices made
- Architectural constraints

## Context (Optional)

[Historical decisions or important background that affects current design]

## Note A (Optional)

[Additional specification-relevant information]
```

### Requirements Files  

システムの横断的な要件や制約、受入基準を定義した文書

#### 形式

```markdown
# [Requirement Name]

[Requirement overview and purpose in 1-2 sentences]

## Details

[Detailed explanation of the requirement]

## Acceptance Criteria

- [Criterion 1]
- [Criterion 2]
- [Criterion 3]

## Context (Optional)

[Background or reasoning for this requirement]

## Note A (Optional)

[Additional information as needed]
```

### Issues Files

実装ギャップや改善が必要な課題、バグ、技術的問題を記録した文書

#### 形式

```markdown
# [Issue Name]

[Issue overview in 1-2 sentences]

## Details

[Detailed explanation of the issue]

## Context (Optional)

[How this issue was discovered or why it needs addressing]

## Note A (Optional)

[Additional information as needed]
```

### Terms Files

プロジェクトで使用する専門用語やドメイン概念の定義と具体例を記述した辞書

#### 形式

```markdown
# [Term Name]

[Concise and accurate definition of the term]

## Examples

[Specific examples or use cases of the term]

## Context (Optional)

[How this term was decided or evolved]

## Note A (Optional)

[Additional information as needed]
```

### Notes Files

開発決定、技術負債、議事録、調査結果、ADRなどの補足情報を記録したメモ文書

#### ファイル命名規約

記述的スラッグ `migration-plan.md`、`database-choice.md`

#### 形式

```markdown
# [Note Title]

[Brief overview of what this note covers]

## Background

[Context and why this note exists]

## Content

[Main content of the note]

## Decisions (Optional)

[Any decisions made if applicable]

## Action Items (Optional)

[Follow-up tasks if applicable]

## Context (Optional)

[Additional historical context]
```
