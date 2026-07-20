# 良いテストと悪いテスト

## 良いテスト

**統合スタイル**: 内部部品のモックではなく、実際のインターフェイスを通じてテストします。

```typescript
// GOOD: Tests observable behavior
test("user can checkout with valid cart", async () => {
  const cart = createCart();
  cart.add(product);
  const result = await checkout(cart, paymentMethod);
  expect(result.status).toBe("confirmed");
});
```

特徴:

- ユーザー/発信者が気にする動作をテストします
- パブリック API のみを使用します
- 内部リファクタリングを生き残る
- 「どのように」ではなく「何を」説明する
- テストごとに 1 つの論理アサーション

## 悪いテスト

**実装の詳細テスト**: 内部構造に結合されています。

```typescript
// BAD: Tests implementation details
test("checkout calls paymentService.process", async () => {
  const mockPayment = jest.mock(paymentService);
  await checkout(cart, payment);
  expect(mockPayment.process).toHaveBeenCalledWith(cart.total);
});
```

危険信号:

- 内部協力者を嘲笑する
- プライベートメソッドのテスト
- コール数/注文のアサート
- 動作を変更せずにリファクタリングするとテストが中断される
- テスト名は、「どのように」ではなく「どのように」を表します。
- インターフェースではなく外部手段を介した検証

```typescript
// BAD: Bypasses interface to verify
test("createUser saves to database", async () => {
  await createUser({ name: "Alice" });
  const row = await db.query("SELECT * FROM users WHERE name = ?", ["Alice"]);
  expect(row).toBeDefined();
});

// GOOD: Verifies through interface
test("createUser makes user retrievable", async () => {
  const user = await createUser({ name: "Alice" });
  const retrieved = await getUser(user.id);
  expect(retrieved.name).toBe("Alice");
});
```

**トートロジー テスト**: 期待値は実装を再表現するため、テストは構築によって合格します。

```typescript
// BAD: Expected value is recomputed the way the code computes it
test("calculateTotal sums line items", () => {
  const items = [{ price: 10 }, { price: 5 }];
  const expected = items.reduce((sum, i) => sum + i.price, 0);
  expect(calculateTotal(items)).toBe(expected);
});

// GOOD: Expected value is an independent, known literal
test("calculateTotal sums line items", () => {
  expect(calculateTotal([{ price: 10 }, { price: 5 }])).toBe(15);
});
```
