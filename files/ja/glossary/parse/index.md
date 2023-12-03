---
title: Parse (解析)
slug: Glossary/Parse
---

{{GlossarySidebar}}

解析(Parsing)とは、プログラムを解析し、例えばブラウザー内の {{glossary("JavaScript")}} エンジンといった実行環境で、実際に実行できる内部形式に変換することを意味します。

[ブラウザーは HTML を解析](/ja/docs/Web/Guide/HTML/HTML5/HTML5_Parser)して {{glossary("DOM")}} ツリーに変換します。HTML の解析処理は[トークン化](/ja/docs/Web/API/DOMTokenList)とツリーの構築を含みます。HTML トークンは、属性の名前や値と同じように、開始タグと終了タグを含みます。文書が適切に構造化されていれば、その解析処理は単純で速くなります。パーサーはトークン化された入力内容を解析してドキュメントに変換し、ドキュメントツリーを作り上げます。

HTML パーサーが画像のようなノンブロッキングのリソースを見つけた場合、ブラウザーはそれらのリソースを要求し、解析を続けます。CSS ファイルに出くわした場合は解析処理を続けることができますが、`<script>` タグ—特に `async` または `defer` 属性がない—の場合はレンダリングがブロックされ、HTML の解析処理が停止します。

ブラウザーは CSS スタイルに出くわした場合、その文字列を解析して CSS Object Model (または {{glossary('CSSOM')}}) に変換し、そのデータ構造をレイアウトの整形と描画に使用します。それからブラウザーは、これら両方のデータ構造から、コンテンツを画面に描画するためのレンダリングツリーを生成します。JavaScript も同様にダウンロードされ、解析され、そして実行されます。

JavaScript では、これは{{glossary("compile time","コンパイル時")}}または、メソッドの呼び出し中など、{{glossary("parser","パーサー")}}が呼び出されるたびに行われます。

## より詳しく知る

### 一般知識

- Wikipedia 上の [Parse](https://en.wikipedia.org/wiki/Parsing)（英語）
