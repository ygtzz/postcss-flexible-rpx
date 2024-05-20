# postcss-flexible-rpx

Forked from [postcss-flexible](https://github.com/crossjs/postcss-flexible)

This is a [postcss](https://www.npmjs.com/package/postcss) plugin for translating`rx`, `rem(...)`, `dpr(...)`, `url(...)`. Similar to [px2rem](https://github.com/songsiqi/px2rem-postcss), but using custom function istead of comments for syntax.

`rpx` is to replace the `rem(...)` because the `rpx` is more concise.
Futhermore, `rx` is to replace `rpx` for concise.

Now, also support `pw` and `ph`, translating to `vw` and `vh`

## Install
```bash
npm i postcss-flexible-rpx -D
```

## Usage

### Webpack

```js
module.exports = {
  module: {
    loaders: [
      {
        test: /\.css$/,
        loader: "style-loader!css-loader!postcss-loader"
      }
    ]
  },
  postcss: function() {
    // remUnit defaults to 75
    return [
      require('postcss-flexible-rpx')({
        pwUnit: 100,
        phUnit: 100,
        remUnit: 75
      })
    ]
  }
}
```

## Example

Before processing:

```css
.selector {
  margin-top: 10ph;
  padding-left: 20pw;
  font-size: dpr(32px);
  width: 75rx;
  height: rem(75px);
  line-height: 3;
  /* replace all @[1-3]x in urls: `/a/@2x/x.png`, `/a@2x/x.png` or `/a/x@2x.png` */
  background-image: url(/images/qr@2x.png);
}
```

After processing:

```css
.selector {
  margin-top: 1vh;
  padding-left: 2vw;
  width: 1rem;
  height: 1rem;
  line-height: 3;
}

[data-dpr="1"] .selector {
  font-size: 16px;
  background-image: url(/images/qr@1x.png);
}

[data-dpr="2"] .selector {
  font-size: 32px;
  background-image: url(/images/qr@2x.png);
}

[data-dpr="3"] .selector {
  font-size: 48px;
  background-image: url(/images/qr@3x.png);
}
```

## options

- `desktop`: boolean, default `false`
- `baseDpr`: number, default `2`
- `pwUnit`: number, default `10`
- `phUnit`: number, default `10`
- `remUnit`: number, default `75`
- `remPrecision`: number, default `6`
- `addPrefixToSelector`: function
- `dprList`: array, default `[3, 2, 1]`
- `fontGear`: array, default `[-1, 0, 1, 2, 3, 4]`
- `enableFontSetting`: boolean, default `false`
- `addFontSizeToSelector`: function
- `outputCSSFile`: function