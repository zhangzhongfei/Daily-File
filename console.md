# 坑爹的console问题记录

> iE6789下面没有console.log()方法  不支持 并且还会报错 

## Q: 取json数据 火狐谷歌浏览器都正常，但是ie8 360兼容模式下都会显示空白，f12之后刷新页面就会出来。

### A:这是因为ie 不支持console.log()，并且会报错，解决方法有两种 1.调试完成后把consoloe.log()都删除掉 2.使用下面的方法

```js
function log(msg) {
  if (window['console']) {
    console.log(msg);
  }
}
```



