---
title: XLink
slug: Glossary/XLink
l10n:
  sourceCommit: 4a9f9d2e9c1811ce3519a7061e906db9200a5e97
---

{{GlossarySidebar}}

XLink は W3C の標準規格で、 XML と、 XML または他の文書との間のリンクを記述するために使用されます。その動作の一部は、どのように処理するかが実装に委ねられています。

単純な XLinks は Firefox で（少なくとも SVG と MathML では）「対応」されていますが、プレーンな XML 文書を XLinks で読み込んで、XML ツリーの関連ポイントをクリックしようとしても、リンクとしては機能しません。

XLink 1.0 で通常のリンクとしては面倒だと感じていた人のために、 XLink 1.1 では、単純なリンクには `xlink:type="simple"` を指定する必要がなくなりました。

XLink は [SVG](/ja/docs/Web/SVG)、[MathML](/ja/docs/Web/MathML)、その他の重要な標準で使用されています。

### 仕様書

- [XLink 1.0](https://www.w3.org/TR/xlink/)
- [XLink 1.1](https://www.w3.org/TR/xlink11/) (現在進行中の草案)

### 関連情報

- [XML](/ja/docs/Web/XML)
- [Code snippets:getAttributeNS](/ja/docs/Web/API/Element/getAttributeNS) - この DOM メソッドに対応していないブラウザーに対応するためのラッパー
