# EsModule

### 开启import

在package.json中增加

```
"type": "module",
```

### 动态绑定

通过import引入的变量会和库中动态绑定；

```javascript
// a.js
let a = Math.random();
setTimeout(() => {
  a = Math.random();
  console.log(a, "a");
}, 200); // 脚本执行后，会在200ms后对a重新赋值
export { a };

// b.js
import { a } from "./a.js";
console.log(a, "in a"); // 首先获取a的值
setTimeout(() => {
  console.log(a, "in a after 1000");
}, 1000);

// result，可以看到两次输出是不一样的
0.6607649293924653 in a
0.10952767527360141 a
0.10952767527360141 in a after 1000
```



