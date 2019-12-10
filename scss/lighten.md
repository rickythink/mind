# lighten

```css
lighten($color, $amount) //=> color 
```

Makes `$color` lighter.

The `$amount` must be a number between `0%` and `100%` \(inclusive\). Increases the HSL lightness of `$color` by that amount.

#### ⚠️ Heads up!

The `lighten()` function increases lightness by a fixed amount, which is often not the desired effect. To make a color a certain percentage lighter than it was before, use [`scale()`](https://sass-lang.com/documentation/modules/color#scale) instead.

Because `lighten()` is usually not the best way to make a color lighter, it’s not included directly in the new module system. However, if you have to preserve the existing behavior, `lighten($color, $amount)` can be written [`adjust($color, $lightness: $amount)`](https://sass-lang.com/documentation/modules/color#adjust).

* [SCSS](https://sass-lang.com/documentation/modules/color#example-16-scss)
* [Sass](https://sass-lang.com/documentation/modules/color#example-16-sass)

#### SCSS SYNTAX

```css
// #e1d7d2 has lightness 85%, so when lighten() adds 30% it just returns white.
@debug lighten(#e1d7d2, 30%); // white

// scale() instead makes it 30% lighter than it was originally.
@debug color.scale(#e1d7d2, $lightness: 30%); // #eae3e0
```

