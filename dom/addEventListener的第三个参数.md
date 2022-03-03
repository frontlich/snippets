```js

target.addEventListener(type, listener, options);
target.addEventListener(type, listener, useCapture);

```

### `options`

warn: 旧版本浏览器可能不支持

```js

{
  capture: Boolean, // 表示`listener`会在该类型的事件捕获阶段传播到该`EventTarget`时触发
  once: Boolean, // 表示`listener`在添加之后最多只调用一次。如果是`true`，`listener`会在其被调用之后自动移除
  passive: Boolean, // 表示`listener`永远不会调用`preventDefault()`。如果`listener`仍然调用了这个函数，客户端将会忽略它并抛出一个控制台警告
  signal：AbortSignal, // 该 AbortSignal 的 abort() 方法被调用时，监听器会被移除。
}

```