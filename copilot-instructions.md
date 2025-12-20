# Copilot Instructions

このリポジトリは **SDD（Spec-Driven Development）** 方式で開発する。  
コードは仕様の結果であり、**仕様が常に最終決定事項（Single Source of Truth）** である。

Copilot は、以下の仕様・設計・タスク・意思決定履歴を  
**コード変更と同時に最新状態へ保つこと** を前提に振る舞うこと。

---

## リポジトリ構成（前提）

```text
repo/
  spec/
    requirements.md   # 要件（EARS 記法）
    design.md         # 設計
    tasks.md          # 実装タスク（進捗管理）

  docs/
    adr/              # ADR（Architecture Decision Record）
      0001-*.md
      0002-*.md
      README.md       # ADR ルール・テンプレ（任意）

  steering/           # 運用ガードレール（SSOT ではない）
    README.md
    coding-style.md
    dev-workflow.md
    testing-quality.md
    security.md
    dependencies.md

  src/                # アプリケーションコード
  tests/              # テスト

  copilot-instructions.md
  README.md
```

- `spec/` 配下は **常に最新・最終決定版（SSOT）**
- `docs/adr/` は **意思決定の履歴（追記のみ）**
- `steering/` は **運用・規約・品質基準を定義するガードレール層**
- コードと仕様が乖離した場合、**仕様が正** とみなす

---

## 仕様ドキュメントの役割

### requirements.md（要件）
- **「何を満たすべきか」** を定義する
- **EARS（Easy Approach to Requirements Syntax）記法**で記載する
- 実装方法や内部構造は書かない（What のみ）

### design.md（設計）
- requirements.md を **どのように満たすか**
- アーキテクチャ、責務分離、データ構造、フロー、エラー方針などを記述
- 実装が design と異なる場合、**設計か実装のどちらかを必ず更新**

### tasks.md（タスク）
- 実装・修正・調査の **実行単位**
- 進捗が分かるようチェックリスト形式で管理
- 次にやるべき作業が常に分かる状態を保つ

---

## Steering（運用ガードレール）について

本リポジトリでは、**SSOT（最終決定版の仕様）とは別レイヤ**として  
**Steering（運用ガードレール）**を設ける。

Steering は、以下を目的とする。

- 開発の進め方・判断基準・禁止事項を固定する
- 人間および Copilot の振る舞いを一貫させる
- 仕様（spec）や設計の内容そのものとは分離する

Steering に含めるのは **運用・規約・品質基準**のみとし、  
要件・設計・タスクなどの**決定事項そのものは spec/ を SSOT とする**。

### Steering の配置と役割

- 配置場所：`steering/`
- Steering は参照・ガイド目的であり、仕様の本文は記載しない
- Steering と spec の内容が矛盾する場合、**spec を正**とする

Copilot は、コード変更や判断を行う際に  
Steering を「行動ルール」、spec を「判断根拠」として扱うこと。

---

## ADR（Architecture Decision Record）運用ルール

- 配置場所：`docs/adr/`
- **意思決定の履歴は上書きしない**
- 判断が変わった場合は **新しい ADR を追加**し、旧 ADR を Superseded にする

### ADR を書くべきタイミング
- 非自明な設計選択を行ったとき
- トレードオフを受け入れる判断をしたとき
- データモデル・公開インターフェースを変更したとき
- 将来の制約や負債を意図的に受け入れたとき

---

## EARS 記法ルール（requirements.md）

各要件は識別子付きで、以下のパターンを用いる。

### パターン
- **常時要件**  
  `The <system> SHALL <response>.`
- **イベント駆動**  
  `WHEN <event>, the <system> SHALL <response>.`
- **状態依存**  
  `WHILE <state>, the <system> SHALL <response>.`
- **例外・好ましくない状況**  
  `IF <condition>, THEN the <system> SHALL <response>.`
- **オプション機能**  
  `WHERE <feature>, the <system> SHALL <response>.`

### ルール
- MUST / SHALL レベルの表現を用いる
- 曖昧な表現を避け、テスト可能であること
- 実装方法・技術名は書かない

---

## 軽微変更の扱いについて

内部実装の整理やリファクタリングなど、  
**外部仕様や振る舞いに影響しない変更**の場合は  
`requirements.md` の更新は不要とする。

ただし、以下に該当する場合は軽微変更とはみなさない。

- 処理フローや責務分担が変わる
- エラー条件・例外ケースの扱いが変わる
- 将来の拡張性・制約に影響する判断を含む

その場合は、`design.md` の更新、または必要に応じて ADR を追加する。

---

## Copilot の振る舞いルール（必須）

### 1. 仕様が存在しない機能は実装しない
- 要件が不明確な場合は、まず requirements.md の案を提示する

### 2. コード変更時の必須確認
コードを変更する場合、以下を必ず確認・更新する。

- 振る舞いが変わる → `requirements.md`
- 構造・設計が変わる → `design.md`
- 作業の進捗が変わる → `tasks.md`
- 判断・方針を決めた → 新しい ADR

### 3. 仕様と実装の不整合がある場合
- 仕様を正として扱う
- 仕様を変える場合は、ADR を伴って明示的に更新する

### 4. 推測で決めない
- 不明点は明示的に不明とし、選択肢と影響を整理する
- 暗黙の判断は ADR として残す

---

## 変更完了条件（Definition of Done）

以下が満たされて初めて「完了」とみなす。

- [ ] requirements.md が最新
- [ ] design.md が最新
- [ ] tasks.md の進捗が反映されている
- [ ] 必要に応じて ADR が追加されている
- [ ] テストが更新・追加されている

---

## 基本方針

- **仕様 → 設計 → タスク → コード** の順序を崩さない
- 仕様は軽くてもよいが、常に存在していること
- 「あとで書く仕様」は存在しない
