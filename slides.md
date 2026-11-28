---
title: 3D MLT はどこに向かっているのか
theme: default
background: /background.png
fonts:
  sans: Noto Sans JP
  mono: Fira Code
  weights: 600,900
class: text-center
comark: true
selectable: false
favicon: /favicon.ico
seoMeta:
 ogImage: https://yutannihilation.github.io/foss4g-hokkaido-2026-slides/thumbnail.png
---

# 3D MLT は<br/>どこに向かっているのか

2026/11/28 FOSS4G Hokkaido 2026

@yutannihilation

---
layout: image-right
image: "/icon.jpg"
backgroundSize: 70%
---

# ドーモ！

## 名前:

湯谷啓明

## 自己紹介:

- 株式会社 MIERUNE で GIS エンジニア見習いをしています。
- 好きな言語は、R と忍殺語です。

---
class: flex flex-col
---

# 3D MLT はどこに向かっているのか？

<div class="my-auto text-center text-7xl">
<v-click>
結論から言うね。<br/>
</v-click>

<v-click>
👉それ、
</v-click>

<v-click>
**わからない**
</v-click>
</div>

---

# MLT（MapLibre Tile）とは

<v-click>

- MVT と同じく、ベクターデータの地図タイル

</v-click>

<v-after>
<figure class="text-center mt-4">
  <img src="/tiles.png" class="mx-auto max-h-80" />
  <figcaption class="text-base mt-4 font-mono">
    （出典：
    <a href="https://maplibre.org/maplibre-tile-spec/overview/" target="_blank" rel="noopener noreferrer">
      Data Model - MapLibre Tile Specification
    </a>
    ）
  </figcaption>
</figure>
</v-after>

---

# MLT（MapLibre Tile）とは

- MVT と同じく、ベクターデータの地図タイル

<v-clicks>

- MVT より**軽くて速い**
  - 非圧縮でも gzip 化した MVT より小さい
  - デコードが2.3～3.7倍高速
- MVT では**表現できなかったデータを扱える**
  - （今日は主にこっちを話します）

</v-clicks>


---
class: flex flex-col
---

# MVT では表現できないデータとは？

<v-click>
<div class="my-auto text-center text-7xl">
3D
</div>
</v-click>

---
layout: iframe
url: https://yutannihilation.github.io/threejs-mlt-road-renderer/
title: 3D MLT の使用例
---

<DemoQr />

---

# MapLibre でこういう 3D をやる選択肢

<v-clicks depth="2">

- MVT
  - 標高タイル、建物の押し出ししかできない
- GeoJSON
  - **サイズが大きい**
  - **タイルになっていないので一気にリクエスト**
- 3D Tiles
  - **オーバースペック**

</v-clicks>

---

# そこで MLT ですよ！

---

# そこで MLT ですよ...?

<figure class="text-center">
  <img src="/zenn.png" class="mx-auto max-h-90" />
  <figcaption class="mt-2 font-mono">
    <a href="https://zenn.dev/mierune/articles/bb8d4eee6b958e" target="_blank" rel="noopener noreferrer">
      https://zenn.dev/mierune/articles/bb8d4eee6b958e
    </a>
  </figcaption>
</figure>

---

# MLT と言っても色々ある

|  | 状況 | 概要 |
|----|--|--|
| v1 | リリース済 | MVT と同等（= **2Dのみ！**） |
| v2 | **絶賛開発中！** | 簡単な 3D に使える |
| v3 | 未定 | 複雑な 3D に使える |

<v-clicks>

- 3D 対応は v2 から
- まだ安定して使えるのは v1 だけ

</v-clicks>

---

# 3D MLT はどこに向かっているのか？

<figure class="text-center mt-4">
  <img src="/3d-concept.jpg" class="mx-auto max-h-100" />
  <figcaption class="text-base mt-4 font-mono">
    （出典：
    <a href="https://www.linkedin.com/posts/maplibre_on-monday-28-september-830-pt-1130-edt-activity-7509229733000241152-zNSt" target="_blank" rel="noopener noreferrer">
        LinkedIn での MapLibre 公式アカウントの投稿
    </a>
    ）
  </figcaption>
</figure>

---

# 3D MLT はどこに向かっているのか？

<v-clicks>

- 目指すもの
  - **カーナビ**みたいなやつ（例：Google Maps Immersive Navigation）
- 目指さないもの：
  - フォトリアリスティック系（**点群データ**とか、**実写のテクスチャ**とか）

</v-clicks>

---

# MLT v2 の 3D とは？

<v-clicks>

- Z 座標
- メッシュ

</v-clicks>

---

# Z 座標

- 点ごとに `x, y, z` が並ぶ（interleaved 方式）
- 標高は $z \times 10^{z\_step}$ で計算される
  - `z_step` は標高の解像度（1mm～10km）

<figure class="text-center mt-4">
  <img src="/vertex.png" class="mx-auto max-h-100" />
  <figcaption class="text-base mt-4 font-mono">
    （出典：
    <a href="https://maplibre.org/maplibre-tile-spec/specification/v2/#z-coordinates" target="_blank" rel="noopener noreferrer">
      MLT v2 (experimental) - MapLibre Tile Specification
    </a>
    ）
  </figcaption>
</figure>

---

# Z 座標

- height mode（最近追加された仕様）
  - `absolute`：平均海面を0とする高さ
  - `relativeToGround`：地表面を0とする高さ

---

# メッシュ？

<v-click>
<figure class="text-center mt-4">
  <img src="/mesh.png" class="mx-auto max-h-100" />
  <figcaption class="text-base mt-4 font-mono">
    （出典：
    <a href="https://github.com/maplibre/maplibre-tile-spec/pull/2026" target="_blank" rel="noopener noreferrer">
      maplibre/maplibre-tile-spec#2026
    </a>
    ）
  </figcaption>
</figure>
</v-click>

---

# メッシュ


- 数週間前に v2 に追加された仕様
- 主に建物に使うことを想定？

<v-clicks>

- MLT v1 の反省「**killer feature がないので流行っていない**」を踏まえて、Z 座標だけでは足りない、という話の流れになったっぽい

</v-clicks>


<img
  src="/stanford-bunny-mesh.png"
  class="absolute top-6 right-8 w-64 pointer-events-none"
  alt="Stanford Bunny Mesh"
/>

---

# みんな大好き PLATEAU も MLT に...？

<figure class="text-center mt-4">
  <img src="/citygml-pr.png" class="mx-auto max-h-100" />
</figure>

---

# みんな大好き PLATEAU も MLT に...？

- サイズ的には、Draco 圧縮ありの 3D Tiles より数%小さくなるらしい
- ただし、実際のレンダリング時のパフォーマンスは、まだレンダラがないので比較できない

---

# メッシュが v2 に入ったということは...？

|  | 状況 | 概要 |
|----|--|--|
| v1 | リリース済 | MVT と同等 |
| v2 | 絶賛開発中！ | 簡単な 3D に使える |
| v3 | 未定 | 複雑な 3D に使える |

---

# じゃあなんすか、v3 くんは無駄だったってことすか？？

|  | 状況 | 概要 |
|----|--|--|
| v1 | リリース済 | MVT と同等 |
| v2 | 絶賛開発中！ | 簡単な 3D に使える |
| **v3** | **未定** | **複雑な 3D に使える** |

---

# v2 に入っていないもの

- マテリアル

<v-click>

（他に思いつかないので、v3 はまじで要らないかもしれない...）

</v-click>

---

# こんなにイケてる感じがするのに<br/> MLT はなぜ流行ってないのか

<v-clicks>

- 正直、ふつうの 2D であれば MVT を置き換えるほどのメリットがない
- 一方、3D はまだ表示する手段すらない
- 理論値を追い求めすぎて実装がむずい（特にv2）

</v-clicks>

---

# 理論値を追い求めすぎて実装がむずい

- v1：TypeScript、Rust、Java、C++
  - サポートしているとは言っても、言語によって実装状況はまちまち
- v2：Rust のみ
  - TypeScript は WASM 経由しかない
  - Java は実装できる人がいないし、レビューもできない

---

# 「もう Rust だけでよくない？」

  <img src="/remove-java1.png" class="mx-auto max-h-100" />

---

# 「そんなわけあるかい😠😠😠」

  <img src="/remove-java2.png" class="mx-auto max-h-100" />

---

# Rust だけしか実装がないことの問題

<v-clicks>

- **信用**：FFI や WASM を介して Rust 実装を使うのが常に受け入れられるわけではない
  - さっきのスライドは「pure-Java じゃないとダメ！」ということで揉めた例
- **速度**：FFI や WASM 越しだとオーバーヘッドがある（例：WASM は JavaScript の文字列が苦手）
- **仕様の頑健性**：単一の実装だけなので、実装のミスや仕様の不備に気付きづらい

</v-clicks>

---

# まとめ

- MLT は v2 で 3D が扱えるようになる
- メッシュも入った
- どこへ向かっているかはわからない
