---
title: Object.prototype.__defineGetter__()
slug: Web/JavaScript/Reference/Global_Objects/Object/__defineGetter__
---

{{JSRef}}

> **警告:** この機能は非推奨となり、[オブジェクト初期化子の構文](/ja/docs/Web/JavaScript/Reference/Operators/Object_initializer)または {{jsxref("Object.defineProperty()")}} API を使用してゲッターを定義する方法で置き換えられました。この機能は広く実装されていますが、古い使い方であるために [ECMAScript 仕様書](https://tc39.github.io/ecma262/#sec-additional-ecmascript-features-for-web-browsers)で非推奨となっています。よりよい代替策が存在するので、このメソッドを使用しないでください。

**`__defineGetter__`** メソッドは、オブジェクトのプロパティと関数を結び付け、そのプロパティが参照されるときに呼び出されるようにします。

## 構文

```js
__defineGetter__(prop, func);
```

### 引数

- `prop`
  - : 関数と結びつけるプロパティの名前を表す文字列です。
- `func`
  - : プロパティの値を参照するときに呼び出される関数です。

### 返値

{{jsxref("undefined")}} です。

## 解説

`__defineGetter__` を使用することで、既存のオブジェクトに{{jsxref("Functions/get", "ゲッター", "", 1)}}を定義することができます。

## 例

### 標準外かつ非推奨の方法

```js
var o = {};
o.__defineGetter__("gimmeFive", function () {
  return 5;
});
console.log(o.gimmeFive); // 5
```

### 標準準拠の方法

```js
// get 演算子を使用
var o = {
  get gimmeFive() {
    return 5;
  },
};
console.log(o.gimmeFive); // 5

// Object.defineProperty を使用
var o = {};
Object.defineProperty(o, "gimmeFive", {
  get: function () {
    return 5;
  },
});
console.log(o.gimmeFive); // 5
```

## 仕様書

{{Specifications}}

## ブラウザーの互換性

{{Compat}}

## 関連情報

- `Object.prototype.__defineGetter__` のポリフィルは [`core-js`](https://github.com/zloirock/core-js#ecmascript-object) で利用できます
- [`Object.prototype.__defineSetter__()`](/ja/docs/Web/JavaScript/Reference/Global_Objects/Object/__defineSetter__)
- {{jsxref("Functions/get", "get")}} 演算子
- {{jsxref("Object.defineProperty()")}}
- [`Object.prototype.__lookupGetter__()`](/ja/docs/Web/JavaScript/Reference/Global_Objects/Object/__lookupGetter__)
- [`Object.prototype.__lookupSetter__()`](/ja/docs/Web/JavaScript/Reference/Global_Objects/Object/__lookupSetter__)
- [JavaScript ガイド: ゲッターとセッターの定義](/ja/docs/Web/JavaScript/Guide/Working_with_objects#ゲッターとセッターの定義)
- [\[Blog
  Post\] Deprecation of \_\_defineGetter\_\_ and \_\_defineSetter\_\_](https://whereswalden.com/2010/04/16/more-spidermonkey-changes-ancient-esoteric-very-rarely-used-syntax-for-creating-getters-and-setters-is-being-removed/)
- [Firefox バグ 647423](https://bugzil.la/647423)
