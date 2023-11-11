---
title: AudioBuffer.numberOfChannels
slug: Web/API/AudioBuffer/numberOfChannels
---

{{ APIRef("Web Audio API") }}

`numberOfChannels` は {{ domxref("AudioBuffer") }} インターフェイスのプロパティで、バッファーに格納された PCM データのチャンネルの数を整数で返します。

## 値

整数です。

## 例

```js
// ステレオ
var channels = 2;

// AudioContext のサンプルレートで2秒間の空のステレオバッファを生成する
var frameCount = audioCtx.sampleRate * 2.0;
var myArrayBuffer = audioCtx.createBuffer(2, frameCount, audioCtx.sampleRate);

button.onclick = function () {
  // バッファにホワイトノイズを書き込む;
  // 単なる -1.0 から 1.0 の間の乱数の値である
  for (var channel = 0; channel < channels; channel++) {
    // 実際のデータの配列を得る
    var nowBuffering = myArrayBuffer.getChannelData(channel);
    for (var i = 0; i < frameCount; i++) {
      // Math.random() は [0; 1.0] である
      // 音声は [-1.0; 1.0] である必要がある
      nowBuffering[i] = Math.random() * 2 - 1;
    }
  }

  console.log(myArrayBuffer.numberOfChannels);
};
```

## 仕様書

{{Specifications}}

## ブラウザーの互換性

{{Compat}}

## 関連情報

- [ウェブオーディオ API の使用](/ja/docs/Web/API/Web_Audio_API/Using_Web_Audio_API)
