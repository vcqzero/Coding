**Vue项目**

但是在vue项目中，在css中引入路径如果也用别名需要在前加上`~`，否则不会被解析；

```scss
background-image: url("~@/baseBg.png");//"~"号代表使用alias @是定义好的src目录
// webpack解析之后为
background-image: url("SRC_PATH/baseBg.png");
```

`~`代表后面的路径使用了别名