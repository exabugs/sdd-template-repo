# Steering（運用ガードレール）

このディレクトリは、本プロジェクトにおける  
**開発の進め方・判断基準・品質ルール**を定義する。

Steering は **SSOT ではない**。  
要件・設計・タスクの最終決定事項は `spec/` を正とする。

---

## 役割

- 人間と Copilot の振る舞いを一貫させる
- 判断基準や禁止事項を明文化する
- 仕様や設計の内容そのものは記載しない

---

## 優先順位

1. 自動強制される設定（lint / formatter / CI）
2. Steering 文書
3. spec/
4. README やコメント

---

## このディレクトリの構成

- coding-style.md：コード規約・フォーマット
- dev-workflow.md：開発フロー・PR ルール
- testing-quality.md：テスト・品質基準
- security.md：セキュリティ方針
- dependencies.md：依存関係ルール
