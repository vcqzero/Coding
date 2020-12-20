开发时，遇到在ios中image组件不显示的问题，需要如下修改

```objective-c
# node_modules/react-native/Libraries/Image/RCTUIImageViewAnimated.m
# 大概267行
- (void)displayLayer:(CALayer *)layer
{
  if (_currentFrame) {
    layer.contentsScale = self.animatedImageScale;
    layer.contents = (__bridge id)_currentFrame.CGImage;
  }else{    //加上这个  不然ios14以上的系统看不见图片
    [super displayLayer:layer];
  }
}
```

