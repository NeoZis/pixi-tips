# Pixi.js Tips & Tricks

During my work on slots, I am faced with different problems when working with pixi.js on some devices sometimes. Here, I want to summarize some of them and also provide possible solutions that work for me.

This repository is mostly for me to accumulate information.

If you have another solution for some problems or want to participate, feel free to create PRs.

## Text

1. Using shadow on some iOS devices.

[Adding `letter-spacing` can help you](https://github.com/pixijs/pixijs/issues/6817).

## Render

1. Screen rotating on iOS devices.

There is issue on some iOS devices with rotating. To trigger re-render our game we usually add `resize` listener, and just use new `width` and `height`.
However on some iOS devices, if you use current screen size, you'll probably get old dimension. This [problem](https://discourse.elm-lang.org/t/incorrect-onresize-window-dimensions-in-some-browsers-on-ios/7232) inspired me to use such solution for pixi.js too and it works very well.

2. Rendering `Graphics` on several Samsung devices

Do you want use `Graphics` as overlay, but then QA creates bug that appears only on some Samsung devices? There is [solution](https://github.com/pixijs/pixijs/issues/8315).

```
const ctx = Texture.WHITE.baseTexture.resource.source.getContext("2d");
ctx.fillRect(0, 0, 1, 1);
```
