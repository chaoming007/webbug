## 移动端常见问题总结
###  1.Safari中设置transform之后z-index不生效
在Safari浏览器下，此Safari浏览器包括iOS的Safari，iPhone上的微信浏览器，以及Mac OS X系统的Safari浏览器，当我们使用3D transform变换的时候，如果祖先元素没有overflow:hidden/scroll/auto等限制，则会直接忽略自身和其他元素的z-index层叠顺序设置，而直接使用真实世界的3D视角进行渲染。

**方法1：**

父级，任意父级，非body级别，设置overflow:hidden可恢复和其他浏览器一样的渲染。

**方法2：**

以毒攻毒。有时候，页面复杂，我们不能给父级设置overflow:hidden，怎么办呢？我们也可以使用3D transform变换。
所以只要设置一个足够大的translateZ值就可以，如100px：
.bar {
    position: fixed;
    z-index: 99;
    /* 此处设置 */
    transform: translateZ(100px);
}

###  2.移动端开发常用设置
1.H5页面窗口自动调整到设备宽度，并禁止用户缩放页面

    <meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no" />

2.忽略将页面中的数字识别为电话号码

    <meta name="format-detection" content="telephone=no" />
3.忽略Android平台中对邮箱地址的识别

    <meta name="format-detection" content="email=no" />
4.viewport模板——通用

    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="utf-8">
    <meta content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no" name="viewport">
    <meta content="yes" name="apple-mobile-web-app-capable">
    <meta content="black" name="apple-mobile-web-app-status-bar-style">
    <meta content="telephone=no" name="format-detection">
    <meta content="email=no" name="format-detection">
    <title>标题</title>
    <link rel="stylesheet" href="index.css">
    </head>
    
    <body>
    这里开始内容
    </body>
    
    </html>


