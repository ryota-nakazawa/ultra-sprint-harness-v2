---
name: grill-lite
description: MVP向けのアイデア、会話、添付資料、既存ドキュメントを短時間で要件整理し、仮定・未解決事項・仕様化の準備状況をMarkdownで出力する。新機能や小規模プロダクトを`to-spec`へ渡す前、または要件確認を長引かせずに進めたいときに使用する。
---

# 軽量要件整理

詳細な仕様書や実装コードは作成しない。後続の `/to-spec` が利用できる、現在の会話内のMarkdown成果物を作成する。

## 情報を集める

次の優先順位で情報を読み、下位の情報で上位の情報を上書きしない。

1. ユーザーの現在の依頼
2. 現在の会話とセッション情報
3. ユーザーが指定したファイル・添付資料
4. リポジトリ内の要件・設計文書（`AGENTS.md`、`CONTEXT.md`、`docs/adr/` を含む）
5. ソースコードと設定ファイル
6. 一般的に妥当な仮定

コードベースや文書で確認できる事項は再質問しない。情報が十分なら質問せず、成果物を出力する。

## 作業対象を判定する

要件を整理したら、成果物タイプを選ぶ前に作業対象を次の二択で判定する。

- `Harness Improvement`: この開発キット自体のスキル、規約、テンプレート、運用文書を変更する仕事。
- `Project Delivery`: この開発キットを使って、利用者向けの成果物を作る仕事。

`Harness Improvement` の場合は、成果物タイプを選ばない。ハーネス変更として、仕様化、チケット化、実装、検証、レビューのフローへ進める。`ハーネス設計` という利用者向け成果物とは混同しない。

作業対象を問わず、意味のある変更ではファイル変更または実装の開始前に専用ブランチで作業する。軽微な誤字、リンク、整形だけの修正は例外にできる。最終出力の `Work Target` に、ブランチの要否と判断理由を記載する。

`Project Delivery` の場合は、次の成果物タイプから主成果物を1つ選ぶ。主成果物を複数選ばず、補助的な出力は付随成果物に記録する。

- `Webアプリ`
- `Agent Skills`
- `GPTsのシステムプロンプト`
- `ハーネス設計`
- `Python／Node.jsスクリプト`
- `Webスクレイピング・ブラウザ自動化`
- `ファイル成果物`

`ハーネス設計` は利用者向けの業務フローや別ハーネスを設計する場合だけに選ぶ。この開発キット自体の変更には選ばない。実装選択肢は成果物タイプと別に記録する。たとえば Playwright はブラウザ自動化の実装選択肢であり、Excel はファイル成果物として付随成果物にできる。

## 判断と質問

情報を「確定事項」「安全に置ける仮定」「未解決事項」に分ける。確定事項と仮定を混ぜない。

質問が必要なのは、回答によってMVPの目的・対象ユーザー・必須機能・成功条件・セキュリティ・データ取扱い・外部連携・大きなコストが大きく変わる場合だけである。一度に重要度順で最大5問を提示し、1問ずつの往復はしない。

次の事項は勝手に決定しない。未解決事項として残し、必要なら `Ready for Spec` を `No` にする。

- 課金、契約、法令、コンプライアンス、個人情報
- データ削除などの不可逆な操作
- 本番環境のセキュリティ判断
- 大きなコストを伴う技術選択
- MVPの目的を大きく変える判断

回答がない場合でも、後から変更可能で安全な不足事項には仮定を置いて処理を続ける。重大な判断が残らない限り、完璧さよりMVPの前進を優先する。

## 成果物を出力する

質問への回答が得られたとき、または質問なしで進められると判断したとき、以下の形式で会話内に出力する。未確認の節は空欄にせず、仮定または未解決事項として記載する。

```markdown
# Grill Result

## Summary

## Work Target

`Harness Improvement` または `Project Delivery`。判定理由、専用ブランチの要否、軽微修正の例外に該当する場合はその理由も記載する。

## Artifact Routing

`Project Delivery` の場合だけ記載する。`Harness Improvement` の場合はこの節を出力しない。

- Primary artifact: 上記の成果物タイプから1つ
- Secondary artifacts: 任意。複数可
- Implementation options: 任意。実装言語、ブラウザ自動化の手段、外部連携など
- Selection rationale: 最小の主成果物を選んだ理由
- Next flow: 主成果物に対応する後続フロー。`Webアプリ` は既存の `/to-spec` 以降の流れへ渡す

## Problem

## Target Users

## User Value

## MVP Scope

## Out of Scope

## Required Capabilities

## Primary User Flow

## Success Criteria

## Constraints

## Confirmed Decisions

## Assumptions

## Open Questions

## Ready for Spec

Yes または No

## Handoff to Spec

`Harness Improvement` または主成果物が `Webアプリ` の `Project Delivery` の場合にだけ出力する。
```

`Harness Improvement` の場合は、`/to-spec` に渡す。

主成果物が `Webアプリ` 以外の `Project Delivery` の場合は、上記の `## Handoff to Spec` を出力せず、代わりに次の節を出力する。

```markdown
## Handoff to Next Flow

`Artifact Routing` の `Next flow` に渡す。
```

`Ready for Spec` は、目的・対象ユーザー・問題・MVPスコープ・必須機能・成功条件が整理され、重大な判断が残っていなければ `Yes` とする。重大な手戻りを招く判断が未解決の場合だけ `No` とする。

`Ready for Spec` が `Yes` の場合、`Harness Improvement` と主成果物が `Webアプリ` の `Project Delivery` は、成果物を現在の会話コンテキストとして `/to-spec` に渡すよう案内する。`Project Delivery` のうち主成果物が `Webアプリ` 以外の場合は、`Artifact Routing` の `Next flow` を案内する。`to-spec` は追加インタビューをせず、この成果物の確定事項・仮定・未解決事項をもとに仕様化する。
