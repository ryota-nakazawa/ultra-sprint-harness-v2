# いつモックするか

**システム境界**のみでモックを作成します。

- 外部 API (支払い、電子メールなど)
- データベース (場合によってはテスト DB を好む)
- 時間/ランダム性
- ファイルシステム (場合によっては)

嘲笑しないでください:

- 独自のクラス/モジュール
- 社内協力者
- あなたがコントロールできるものすべて

## モック可能性を考慮した設計

システム境界では、モックしやすいインターフェイスを設計します。

**1.依存性注入を使用する**

内部で依存関係を作成するのではなく、外部の依存関係を渡します。

```typescript
// Easy to mock
function processPayment(order, paymentClient) {
  return paymentClient.charge(order.total);
}

// Hard to mock
function processPayment(order) {
  const client = new StripeClient(process.env.STRIPE_KEY);
  return client.charge(order.total);
}
```

**2.汎用フェッチャーよりも SDK スタイルのインターフェースを優先します**

条件付きロジックを備えた 1 つの汎用関数ではなく、外部操作ごとに特定の関数を作成します。

```typescript
// GOOD: Each function is independently mockable
const api = {
  getUser: (id) => fetch(`/users/${id}`),
  getOrders: (userId) => fetch(`/users/${userId}/orders`),
  createOrder: (data) => fetch('/orders', { method: 'POST', body: data }),
};

// BAD: Mocking requires conditional logic inside the mock
const api = {
  fetch: (endpoint, options) => fetch(endpoint, options),
};
```

SDK アプローチとは次のことを意味します。
- 各モックは 1 つの特定の形状を返します
- テスト設定に条件付きロジックがありません
- テストがどのエンドポイントを実行するかを簡単に確認できます
- エンドポイントごとのタイプセーフティ
