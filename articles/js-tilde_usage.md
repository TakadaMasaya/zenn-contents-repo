---
title: "【JavaScript】なんやこの記法...！「~indexOf()」ビット否定演算子"
emoji: "🐍"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["javascript"]
published: true
---

## なんやこの記法...！！
ある日の業務中、チームメンバーが書いた以下のようなコードが目に止まりました。  
(なんやこの記法...！ `String.indexOf()` の前にチルダ `~` がついとるで...！)

```javascript
if (~text.indexOf(searchText)) {
  return something
}
```

大変お恥ずかしながら、自分で書いたことはおろか見たこともなく、何が行われているのか全くわかりませんでした。

## 結論
JavaScriptのチルダ `~` はビット否定演算子で、上記のコードでは `indexOf()` の返り値の各ビットを反転しているのでした。

> # ビット否定 (~)
> ビット否定演算子 (~) は、オペランドの各ビットを反転します。他のビット演算子と同様、オペランドを 32 ビット符号付き整数に変換します。

https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Bitwise_NOT

ビット否定することにより、[`indexOf()` の返り値](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/String/indexOf#%E8%BF%94%E5%80%A4) のうち、見つからなかった場合の `-1` だけが `0` 、つまり Falsy な値になるというものでした。

:::details 詳しく

```javascript
const searchIndex = text.indexOf(searchText)
const found = ~searchIndex

if (found) {
  return text
}
```

1. テキスト内を検索
    検索結果として、それぞれ以下の値が代入
    - 見つかった → 0 以上の整数
    - 見つからなかった → -1
    ```javascript
    const searchIndex = text.indexOf(searchText) 
    ```
2. 検索結果のインデックスをビット否定
    ビット否定されることにより、以下のように変換
    - -1 → 0 (Falsy)
    - -1以外 → 0以外の整数 (Truthy)
    ```javascript
    const found = ~searchIndex
    ```
3. ビット否定した結果をもとに条件分岐
    ```javascript
    if (found) {
      return text
    }
    ```

:::

## まとめ
JavaScript のチルダ `~` はビット否定演算子。
`String.indexOf()` 関数 (`Array.indexOf()` も同様) と合わせて使用すると、探しものが見つからなかった場合にだけFalsyな値を作る、ということが容易になります。

個人的には、普通にこっちのほうが直感的でわかりやすいように感じました。
```javascript
if (text.indexOf(searchText) !== -1) {
  return text
}
```

皆様のご意見お待ちしております。
記事の中に誤った情報がありましたら、優しくご指摘いただけますと幸いでございます。
