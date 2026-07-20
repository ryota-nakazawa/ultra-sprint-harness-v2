# Ultra Sprint Harness v2

仕様化から実装、レビューまでを短い反復で進めるための、日本語のエージェント開発キットです。

## 含まれるスキル

```text
grill-with-docs
  → to-spec
  → to-tickets
  → implement（必要に応じて tdd）
  → code-review
```

- `grill-with-docs`: 要件を質問で明確にし、`CONTEXT.md` と ADR に残します。
- `to-spec`: 合意済みの会話を仕様書と GitHub Issue に変換します。
- `to-tickets`: 仕様を、単独で検証できる縦方向の実装チケットへ分けます。
- `implement`: 仕様またはチケットに従い、テストと型検査を行って実装します。
- `tdd`: 合意済みの公開境界で、赤・緑の小さな反復を行います。
- `code-review`: 標準適合と仕様適合を、並列サブエージェントで別々にレビューします。

このMVPには `grilling` を含めません。質問が必要な場合は `grill-with-docs` が担当します。

## 使い方

1. このリポジトリの `.agents/skills/` を、利用するプロジェクトの `.agents/skills/` に配置します。
2. ルートの `AGENTS.md` と `docs/agents/` をプロジェクトの運用に合わせて配置・更新します。
3. GitHub Issues を使う場合は、`ready-for-agent` などのラベルを作成します。
4. 新しい機能では、上記の順序でスキルを呼び出します。

## 前提

- GitHub Issues を使う場合は `gh` CLI にログイン済みであること。
- `code-review` は並列サブエージェントを利用できる環境で実行すること。
- 仕様・チケット・ADR・用語集はすべて日本語で記録すること。
