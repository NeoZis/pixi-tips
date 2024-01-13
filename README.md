# Pixi.js Tips & Tricks

During my work on slots, I am faced with different problems when working with pixi.js on some devices sometimes. Here, I want to summarize some of them and also provide possible solutions that work for me.

This repository is mostly for me to accumulate information.

If you have another solution for some problems or want to participate, feel free to create PRs.

## Text

1. Using shadow on some iOS devices.

[Adding `letter-spacing` can help you](https://github.com/pixijs/pixijs/issues/6817).

## Render

1. Screen rotating on iOS devices.

An issue arises on some iOS devices with rotating.
To trigger a re-render in our game, we usually add a resize listener and use the new width and height.
However, on some iOS devices, using the current screen size may result in obtaining the old dimensions.
This [problem](https://discourse.elm-lang.org/t/incorrect-onresize-window-dimensions-in-some-browsers-on-ios/7232) inspired me to implement a similar solution for pixi.js, and it works very well.

2. Rendering `Graphics` on several Samsung devices

Do you want to use Graphics as an overlay? However, a bug appears only on some Samsung devices when QA creates it. There is a [solution](https://github.com/pixijs/pixijs/issues/8315).

```
const ctx = Texture.WHITE.baseTexture.resource.source.getContext("2d");
ctx.fillRect(0, 0, 1, 1);
```
