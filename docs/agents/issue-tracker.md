# ローカルチケット管理

仕様書と実装チケットはローカルのMarkdownで管理する。仕様書は `.scratch/<機能名>/spec.md`、実装チケットは `.scratch/<機能名>/tickets/<NN>-<slug>.md` に保存する。

## 操作の規約

- 仕様は `.scratch/<機能名>/spec.md` に作成し、`/to-tickets` の入力として渡す。
- 実装チケットは `.scratch/<機能名>/tickets/` に依存関係が分かる順で作成し、本文にブロッカーを記載する。
- `.scratch/` はローカル成果物置き場であり、Git 管理しない。
