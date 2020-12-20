# Touch事件

touch事件类似与pc端的鼠标事件，是移动端独有的事件，最早出现在ios safari浏览器，后来Android浏览器也进行了支持，目前所有移动端的浏览器包括webview均支持touch事件。



## PC端鼠标事件

在pc端浏览器中，点击一次鼠标触发的事件如下：

```javascript
mousedown -> mouseup -> click // 在事件event中可以获得到鼠标按键信息
```

鼠标事件对象中保存着鼠标相对于视口、页面和屏幕的坐标位置信息，通过这些信息可以精确地对鼠标事件进行控制。

+ clientX、clientY：鼠标事件发生时鼠标指针在视口（可以看见的区域）中的水平坐标和垂直坐标

+ pageX、pageY：鼠标事件发生时鼠标指针相对于整个页面的位置坐标

+ screenX、screenY：鼠标事件发生时鼠标指针相对于整个屏幕的坐标信息

mousedown和mouseup事件的event对象中存在着一个button属性，表示按下或释放的按钮。DOM的button属性有3个值：

- 0：表示主鼠标按钮（鼠标左键）

- 1：表示中间的鼠标按钮（鼠标滚轮）

- 2：次鼠标按钮（鼠标右键）

  

## 移动端click延迟 点击穿透

**点击延迟**： 在移动端浏览器有独有的双击和长按事件，移动端浏览器为了判断单击和双击，在单击时会延迟300ms，在此时间内，如果再次点击则为双击事件，如果没有点击，则为单击事件。这就是延迟的由来。

**点击穿透**：当一个div上有一个modal,当点击modal时，modal会消失，此时如果modal绑定touch事件，div绑定了div事件，当用户点击时，modal立刻消失，300ms后，div的点击事件被触发。

## 移动端点击事件

```javascript
touchstart > touchmove > touchend > 300ms > mousedown > mouseup > click
```

## 解决方案

以下可以通过 hack 技巧，不添加 fastClick 也能修复延迟的问题

### 禁用缩放

- Chrome on Android (all versions)
- iOS 9.3

```
<meta name="viewport" content="user-scalable=no" />
复制代码
```

或者

```
html {
  touch-action: manipulation;
}
复制代码
```

- IE on Windows Phone

```
html {
  touch-action: manipulation; // IE11+
  -ms-touch-action: manipulation; // IE10
}
复制代码
```

### 不禁用缩放

- Chrome 32+ on Android
- iOS 9.3

```
<meta name="viewport" content="width=device-width" />
复制代码
```

经测试，如果不添加 `width=device-width` 不管是 Android 还是 iOS 在已修复的版本中仍然会出现延时的问题。



## 总结

如果是在android webview中使用，可以禁用缩放，屏蔽300ms

如果是ios 9.3以上的WKWebview,  可以禁用缩放，屏蔽300ms

如果需要适配其他浏览器，需要考虑引入fastclick