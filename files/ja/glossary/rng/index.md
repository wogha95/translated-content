---
title: Random Number Generator (乱数発生器)
slug: Glossary/RNG
---

{{GlossarySidebar}}

**PRNG** (擬似乱数発生器) は、複雑で、予測できないように見えるパターンの数字を出力するためのアルゴリズムです。真の乱数 (放射性線源など) はまったく予測できないのに対し、すべてのアルゴリズムは予測可能なので、 PRNG は、開始パラメーターや*シード*が同じときには同じ数値を返します。

疑似乱数はゲームなど、様々な応用分野で使用されています。

暗号学的に安全な擬似乱数とは、暗号での使用に合うよう追加のプロパティを伴う擬似乱数であり、次のようなものがあります。

- (シードを知らずに) 攻撃者が計算によって出力を予測することができないもの
- 攻撃者がその現在の状態を解くことができた場合、攻撃者が以前に発行された数を解明することができないもの。

多くの疑似乱数、は暗号学的に安全ではありません。

## 関連情報

### 一般知識

- Wikipedia の[擬似乱数](https://ja.wikipedia.org/wiki/擬似乱数)
- JavaScript の組み込み PRNG 関数である {{jsxref("Math.random()")}}。これは暗号学的に安全な PRNG ではありません。
- {{domxref("Crypto.getRandomValues()")}}: 暗号学的に安全な数値を提供するためのものです。
- {{domxref("RandomSource")}}
- {{jsxref("Math.random()")}}
- {{domxref("Crypto.getRandomValues()")}}
