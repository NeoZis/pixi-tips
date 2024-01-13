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

Do you want to use Graphics as an overlay? However, a bug was created by the QA engineer in Jira, with a description stating 'appears only on some Samsung devices.' Fortunately, there is a [solution](https://github.com/pixijs/pixijs/issues/8315).

```
const ctx = Texture.WHITE.baseTexture.resource.source.getContext("2d");
ctx.fillRect(0, 0, 1, 1);
```
## Spine

Of course examples in [pixijs/spine](https://github.com/pixijs/spine/blob/master/examples/index.md) are very useful.

1. Attaching custom container to Spine's slot

```
  attachToSlot(slot: string, object: DisplayObject, removeChildren = true, useFirstChildPosition = false) {
      const slotIndex = this.skeleton.findSlotIndex(slot);
      if (slotIndex === -1) console.error(`There is no slot with name ${slot} in animation.`);
  
      const slotContainer = this.slotContainers[slotIndex];
      const container = new Container();
  
      if (useFirstChildPosition) {
        container.position.copyFrom(slotContainer.children[0].position);
      }
  
      if (removeChildren) {
        slotContainer.removeChildren();
      }
  
      container.addChild(object);
  
      container.scale.y = -1;
  
      slotContainer.addChild(container);
   }
```

2. Change skin

```
  changeSkin(name: string): void {
    this.skeleton.setSkin(null);
    this.skeleton.setSlotsToSetupPose();
    this.skeleton.setSkinByName(name);
  }
```

3. Working with spine events as pixi events

As for me it's very convient way:
```
  this.state.addListener({
    start: (...data) => {
      this.emit('start', ...data);
    },
    complete: (...data) => {
      this.emit('complete', ...data);
    },
    event: (...data) => {
      this.emit('event', ...data);
    },
  });
```

And then you can use it like here:
```
  this.spine.once('event', (_, event) => {
    if (event.data.name === 'my_custom_event') {
      // run some code
    }
  });
```
