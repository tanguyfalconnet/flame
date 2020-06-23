# Layers

Layers are an useful feature that lets you group renderization by context, and to also to obtain a way of pre rendering things, enabling for example the renderization in memory of parts of your game that don't change much, like a background for example, and by doing that, freeing resources for more dynamic content that needs to be rendered every loop cycle.

There are two types of layers on Flame: `DynamicLayer` and `PreRenderedLayer`.

## DynamicLayer

Dynamic layers are layers that are rendered every time that they are draw on the canvas, as the name suggests, it is meant for dynamic content and is most useful to group renderizations that are of the same context.

Usage example:
```dart
class GameLayer extends DynamicLayer {
  final MyGame game;

  GameLayer(this.game);

  @override
  void drawLayer() {
    game.playerSprite.renderRect(
      canvas,
      game.playerRect,
    );
    game.enemySprite.renderRect(
      canvas,
      game.enemyRect,
    );
  }
}

class MyGame extends Game {
  // Other methods ommited...

  @override
  void render(Canvas canvas) {
    gameLayer.render(canvas); // x and y can be provided as optional position arguments
  }
}
```

## PreRenderedLayer

Pre rendered layers are layers that are rendered only once, cached on the memory and they can be used to be rendered on the game canas aftwards. They are most uesful to cache content that don't change during the game, like a background for example.

Usage example:
```dart
class BackgroundLayer extends PreRenderedLayer {
  final Sprite sprite;

  BackgroundLayer(this.sprite);

  @override
  void drawLayer() {
    sprite.renderRect(
      canvas,
      const Rect.fromLTWH(50, 200, 300, 150),
    );
  }
}

class MyGame extends Game {
  // Other methods ommited...

  @override
  void render(Canvas canvas) {
    backgroundLayer.render(canvas); // x and y can be provided as optional position arguments
  }
}
```

## Layer Processors

Flame also provides a way to add processors on your layer, which are ways to add effects on all of your layer. At the moment, out of the box, only the `ShadowProcessor` is available, this processor renders a cool back drop shadow on your layer.

To add processors to your layer, just add them to the layer `preProcessors` or `postProcessors` list. For example:

```dart
// Works the same for both DynamicLayer and PreRenderedLayer
class BackgroundLayer extends PreRenderedLayer {
  final Sprite sprite;

  BackgroundLayer(this.sprite) {
    preProcessors.add(ShadowProcessor());
  }

  @override
  void drawLayer() { /* ommited */ }
```

Custom processors can be creted by extending the `LayerProcessor` class.

You can check an working example of layers [here](/doc/examples/layers)