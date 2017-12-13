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

###  2.禁止蒙层底部页面跟随滚动
方案一
打开蒙层时，给 body 添加样式：

    overflow: hidden;
    height: 100%;
在某些机型下，你可能还需要给根节点添加样式：

    overflow: hidden;

优点： 简单方便，只需添加 css 样式，没有复杂的逻辑。

缺点： 兼容性不好，适用于 pc，移动端就尴尬了。 部分安卓机型以及 safari 中，无法无法阻止底部页面滚动。

方案二
就是利用移动端的 touch 事件，来阻止默认行为（这里可以理解为页面滚动就是默认行为）。

    // node为蒙层容器dom节点
    
    node.addEventListener('touchstart', e => {
    
      e.preventDefault()
    
    }, false)
简单粗暴，滚动时底部页面也无法动弹了。假如你的蒙层内容不会有滚动条，那么上述方法 prefect。

方案三
来讲讲我的思路，既然我们要阻止页面滚动，那么何不将其固定在视窗（即 position: fixed），这样它就无法滚动了，当蒙层关闭时再释放。 当然还有一些细节要考虑，将页面固定视窗后，内容会回头最顶端，这里我们需要记录一下，同步 top 值。

    let bodyEl = document.body
    
    let top = 0
    
    
    function stopBodyScroll (isFixed) {
    
      if (isFixed) {
    
        top = window.scrollY
    
    
        bodyEl.style.position = 'fixed'
    
        bodyEl.style.top = -top + 'px'
    
      } else {
    
        bodyEl.style.position = ''
    
        bodyEl.style.top = ''
    
    
        window.scrollTo(0, top) // 回到原先的top
    
      }
    
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


5.移动端如何定义字体font-family

中文字体使用系统默认即可，英文用Helvetica

/* 移动端定义字体的代码 */
body{font-family:Helvetica;}

6.当用户手指放在移动设备在屏幕上滑动会触发的touch事件

以下支持webkit
touchstart——当手指触碰屏幕时候发生。不管当前有多少只手指
touchmove——当手指在屏幕上滑动时连续触发。通常我们再滑屏页面，会调用event的- - - preventDefault()可以阻止默认情况的发生：阻止页面滚动
touchend——当手指离开屏幕时触发
touchcancel——系统停止跟踪触摸时候会触发。例如在触摸过程中突然页面alert()一个提示框，此时会触发该事件，这个事件比较少用
TouchEvent
touches：屏幕上所有手指的信息
targetTouches：手指在目标区域的手指信息
changedTouches：最近一次触发该事件的手指信息
touchend时，touches与targetTouches信息会被删除，changedTouches保存的最后一次的信息，最好用于计算手指信息

6.ios系统中元素被触摸时产生的半透明灰色遮罩怎么去掉

ios用户点击一个链接，会出现一个半透明灰色遮罩, 如果想要禁用，可设置-webkit-tap-highlight-color的alpha值为0，也就是属性值的最后一位设置为0就可以去除半透明灰色遮罩

    a,button,input,textarea{-webkit-tap-highlight-color: rgba(0,0,0,0;)}
	
7.webkit表单元素的默认外观怎么重置

    .css{-webkit-appearance:none;}

8.webkit表单输入框placeholder的颜色值能改变么

    input::-webkit-input-placeholder{color:#AAAAAA;}
    input:focus::-webkit-input-placeholder{color:#EEEEEE;}
	
9.禁用 radio 和 checkbox 默认样式

::-ms-check 适用于表单复选框或单选按钮默认图标的修改，同样有多个属性值，设置它隐藏 (display:none) 并使用背景图片来修饰可得到我们想要的效果。

    input[type=radio]::-ms-check,input[type=checkbox]::-ms-check{
    display: none;
    }
10.禁止ios 长按时不触发系统的菜单，禁止ios&android长按时下载图片

    .css{-webkit-touch-callout: none}
11.禁止ios和android用户选中文字

    .css{-webkit-user-select:none}

12.屏幕旋转的事件和样式

    window.onorientationchange = function(){
        switch(window.orientation){
            case -90:
            case 90:
            alert("横屏:" + window.orientation);
            case 0:
            case 180:
            alert("竖屏:" + window.orientation);
            break;
        }
    }
	
    //竖屏时使用的样式
    @media all and (orientation:portrait) {
    .css{}
    }
    
    //横屏时使用的样式
    @media all and (orientation:landscape) {
    .css{}
    }
	
12.audio元素和video元素在ios和andriod中无法自动播放

    $('html').one('touchstart',function(){
        audio.play()
    })
	
13.ios使用-webkit-text-size-adjust禁止调整字体大小

    body{-webkit-text-size-adjust: 100%!important;}
	
14.解决滑动卡顿的样式

     -webkit-overflow-scrolling: touch;


15.页面顶部阴影

下面这个简单的 CSS3 代码片段可以给网页加上漂亮的顶部阴影效果：

    body:before {
    
              content: "";
    
              position: fixed;
    
              top: -10px;
    
              left: 0;
    
              width: 100%;
    
              height: 10px;
    
    
              -webkit-box-shadow: 0px 0px 10px rgba(0,0,0,.8);
    
              -moz-box-shadow: 0px 0px 10px rgba(0,0,0,.8);
    
              box-shadow: 0px 0px 10px rgba(0,0,0,.8);
    
    
              z-index: 100;
    
    }