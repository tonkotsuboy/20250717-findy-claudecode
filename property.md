# 6.`@property`

CSS変数に型の情報を与えられる & CSS変数を使ったアニメーションが可能になる

## 基礎編①: ボックスの横幅のCSS変数を `@property` で管理する

```html
<div class="box"></div>
```

```css
@property --box-width {
  syntax: "<length>"; /* 値の型。長さ・色・角度などを指定可能 */
  inherits: false;  /* 継承が可能かどうか？ trueにすると、変数の値が上書き可能になる */
  initial-value: 100px; /* 初期値 */
}

.box {
  width: var(--box-width);
}
```

`syntax` に使える主な値

| syntax | 説明 |
| --- | --- |
| `<length>` | 長さ（px, em, rem など） |
| `<number>` | 数値 |
| `<percentage>` | パーセント値 |
| `<length-percentage>` | 長さまたはパーセント値 |
| `<color>` | 色 |
| `<image>` | 画像 |
| `<url>` | URL |
| `<integer>` | 整数 |
| `<angle>` | 角度（deg, rad など） |
| `<time>` | 時間（s, ms） |
| `<resolution>` | 解像度（dpi, dpcm など） |
| `<transform-function>` | 変形関数（rotate, scale など） |
| `<custom-ident>` | カスタム識別子 |
- `+` :スペース区切りの値
- `#` カンマ区切りの値

## 基礎編②: CSS変数を使ったアニメーション

- `.box` の横幅を、200pxまで伸ばすアニメーションをする

```css
@property --box-width {
  syntax: "<length>";
  inherits: false;
  initial-value: 200px;
}

.box {
  width: var(--box-width);
  animation: 1s box-animation infinite alternate;
}

@keyframes box-animation {
  to {
    --box-width: 400px;
  }
}
```

## 基礎編③: グラデーションのアニメーション

- `conic-gradient`
    - 円錐状のグラデーション
    - `conic-gradient(from 開始角度, 中間色1, 中間色2, ...);`

```html
<div class="box"></div>
```

```css

@property --angle {
  syntax: "<angle>";
  inherits: false;
  initial-value: 0deg;
}

.box {
  width: 400px;
  height: 400px;
  background: conic-gradient(from var(--angle), red, blue);
  animation: 3s angle-animation infinite linear;
}

@keyframes angle-animation {
  to {
    --angle: 1turn;
  }
}

```

## 実践編①: 虹色ボーダーアニメーションボタン

- ボタンのボーダーがグラデーションになっている
- グラデーションがアニメーションしている

[CleanShot 2024-11-28 at 10.05.27-converted.mp4](https://prod-files-secure.s3.us-west-2.amazonaws.com/57c93bc7-61f5-4abe-b744-54f599fec8e7/dacdf9b0-1800-44d5-8836-52fd220da206/CleanShot_2024-11-28_at_10.05.27-converted.mp4)

### IE時代

- ボタンアニメーションを動画やアニメgifで作成して貼り付ける
    - テキストを差し替えるたびに動画を作り直す👎
    - レスポンシブデザインに対応できない👎
    - マウスホバー時にだけアニメーションをするなど、マイクロインタラクションに対応できない👎
    - ファイルサイズが重い👎

### イマドキCSS

- HTMLとCSSだけで実現可能
    - テキストはすぐに差し替えられる👍
    - インタラクションも手軽に作れる👍
    - ファイルサイズが軽い👍

```html
<button class="rainbow-border-animation">虹色のボタン</button>
```

```css
/* CSS変数の定義 */
@property --gradient-angle {
  syntax: "<angle>"; /* 角度を扱う */
  initial-value: 0deg;
  inherits: false;
}

.rainbow-border-animation {
  /* ボーダーを透明にしておき、グラデーションが描画されるようにする */
  border: 6px solid transparent;
  /* background-imageを,でつなげて複数指定する */
  background-image: 
    /* 中心の青い部分 */
    linear-gradient(navy, navy),
    /* 外側ボーダーの虹色グラデーション */
    conic-gradient(
      /* グラデーションの開始位置は定義したCSS変数 */
      from var(--gradient-angle),
      hsl(0, 99%, 57%) 0%,
      hsl(90, 99%, 57%) 25%,
      hsl(180, 99%, 57%) 50%,
      hsl(270, 99%, 57%) 75%,
      hsl(360, 99%, 57%) 100%
    );
  /* 中心の青い部分がボーダー内側に広がり、グラデーションがボーダー部分に広がるようにする */
  background-clip: padding-box, border-box;
  /* 中心の青い部分がボーダー内側から開始し、グラデーションがボーダー外側から開始するようにする */
  background-origin: padding-box, border-box;
  /* CSS変数をアニメーションさせる */
  animation: 2s gradient-angle infinite linear;
}

@keyframes gradient-angle {
  to {
    /* グラデーションの開始位置を360度まで広げるようにする。1turnでもOK */
    --gradient-angle: 360deg;
  }
}
```

- background-origin
    - 背景の開始位置
    https://developer.mozilla.org/ja/docs/Web/CSS/background-origin
- background-clip
    - 背景をどこまで拡張するか？

## 実践編②: フォーカスするとキラキラ光るカード



- カードにフォーカスすると、周りのボーダーが光る

```html
<a class="card" href="#">
  <img alt="Owl" height="40" src="./images/owl.svg" width="40" />
  <h2>Owl</h2>
  <p>
    A nocturnal bird that flies silently. Known for its large eyes, sharp claws,
    and as a symbol of wisdom.
  </p>
</a>
```

### イマドキCSS

```css
@property --gradient-angle {
  syntax: "<angle>";
  initial-value: 0turn;
  inherits: false;
}

.card:is(:focus, :hover) {
  animation: 2s gradient-angle infinite linear;
  border: 2px solid transparent;
  background-image: linear-gradient(#584827, #2d230f),
    conic-gradient(
      from var(--gradient-angle),
      #584827 0%,
      #c7a03c 37%,
      #f9de90 30%,
      #c7a03c 33%,
      #584827 40%,
      #584827 50%,
      #c7a03c 77%,
      #f9de90 80%,
      #c7a03c 83%,
      #584827 90%
    );
  background-clip: padding-box, border-box;
  background-origin: padding-box, border-box;
}

@keyframes gradient-angle {
  to {
    --gradient-angle: 1turn;
  }
}
```

https://codepen.io/tonkotsuboy/pen/LEPWYzP


## ブラウザ対応状況

[CSS at-rule: `@property` | Can I use... Support tables for HTML5, CSS3, etc](https://caniuse.com/mdn-css_at-rules_property)

`@property` 2024年2月から全ブラウザ対応

[types: `<gradient>`: `conic-gradient()` | Can I use... Support tables for HTML5, CSS3, etc](https://caniuse.com/mdn-css_types_gradient_conic-gradient)

`conic-gradient` 2020年から全ブラウザ対応