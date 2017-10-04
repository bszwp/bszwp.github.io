---
layout: post
title: "自定义(滑动条)input[type='range']样式"
date: 2017-01-11
tags: CSS
---



Range是[HTML5](http://lib.csdn.net/base/html5)中新出现的滑块控件，也是常见的控件的之一，不过这个控件的原始样式略丑，所以想对它进行一些改造。需要注意的是Internet Explorer 9及更早IE版本并不支持这个控件。 

### 1、如何使用滑动条？

用法很简单，如下所示：

```html
<input type="range" value="0">
```

各浏览器原始样式如下： 
Chrome: ![这里写图片描述](http://img.blog.csdn.net/20160602004354859) 
Firefox:  ![这里写图片描述](http://img.blog.csdn.net/20160602100431440) 
IE 9+:      ![这里写图片描述](http://img.blog.csdn.net/20160602005252347) 
常用（部分）属性如下：

| 属性           | 描述                       |
| ------------ | ------------------------ |
| max          | 设置或返回滑块控件的最大值            |
| min          | 设置或返回滑块控件的最小值            |
| step         | 设置或返回每次拖动滑块控件时的递增量       |
| value        | 设置或返回滑块控件的 value 属性值     |
| defaultValue | 设置或返回滑块控件的默认值            |
| autofocus    | 设置或返回滑块控件在页面加载后是否应自动获取焦点 |

### 2、如何美化滑动条？

首先提一个问题有哪些方式能完成对滑动条的美化？目前我所能想到的就是如下的两种方案：

- **直接通过css完成样式改造**
- **将滑动条隐藏(设置opacity: 0)，通过自定义div实现**

这次所要介绍的第一种较为简单的实现方式。 

### 3、具体的实现方案是什么？

美化滑动控件，需要完成以下的五个步骤：

- **去除系统默认的样式；**
- **给滑动轨道(track)添加样式；**
- **给滑块(thumb)添加样式；**
- **根据滑块所在位置填充进度条；**
- **实现多浏览器兼容。**

以上就是实现滑动控件美化的整个流程。我们今天所要达到的效果是这样的：![这里写图片描述](http://img.blog.csdn.net/20160602211520922)如果想要实现填充进度条的效果，在IE 9以上的浏览器中可以使用::-ms-fill-lower 和 ::-ms-fill-upper来自定义进度条；在Firefox浏览器中则可以通过::-moz-range-progress来自定义；而Chrome浏览器中不支持直接设置进度条，而达到填充的效果，所以首先针对Chrome浏览器来实现整个流程。

#### 3.1 去除系统默认的样式

这是美化滑动控件的第一步，这步操作是为了不使用原有的样式，使之后的自定义样式有效。代码很简单如下所示，不过要注意的是对基于 webkit 的浏览器，如Chrome, Safari, Opera等，滑块也要移除默认样式。

```css
input[type=range] {
  	/* 去除滑槽默认样式 */
    -webkit-appearance: none;
    width: 300px;
    border-radius: 10px; /*这个属性设置使填充进度条时的图形为圆角*/
}
input[type=range]::-webkit-slider-thumb {
  	/* 去除滑块默认样式 */
    -webkit-appearance: none;
}
```

#### 3.2 给滑动轨道(track)添加样式

正式开始自定义控件样式了。首先是自定义滑动控件的轨道，代码很简单，直接贴出来。

```css
input[type=range]::-webkit-slider-runnable-track {
    height: 15px;
    border-radius: 10px; /*将轨道设为圆角的*/
    box-shadow: 0 1px 1px #def3f8, inset 0 .125em .125em #0d1112; /*轨道内置阴影效果*/
}
```

原始的控件获取到焦点时，会显示包裹整个控件的边框，所以还需要把边框取消。

```css
input[type=range]:focus {
    outline: none;
}
```

#### 3.3 给滑块(thumb)添加样式

下面对滑块的样式进行变更，css代码也不是很复杂，如下所示：

```css
input[type=range]::-webkit-slider-thumb {
    -webkit-appearance: none;
    height: 25px;
    width: 25px;
    margin-top: -5px; /*使滑块超出轨道部分的偏移量相等*/
    background: #ffffff; 
    border-radius: 50%; /*外观设置为圆形*/
    border: solid 0.125em rgba(205, 224, 230, 0.5); /*设置边框*/
    box-shadow: 0 .125em .125em #3b4547; /*添加底部阴影*/
}
```

#### 3.4 根据滑块所在位置填充进度条

新建一个RangeSlider.js文件，实现对滑动控件属性的设置、事件的监听、以及设置回调函数。监听input事件时，对进度条进行填充，让我们来直接看看代码是怎么实现的。

```js
$.fn.RangeSlider = function(cfg){
    this.sliderCfg = {
        min: cfg && !isNaN(parseFloat(cfg.min)) ? Number(cfg.min) : null, 
        max: cfg && !isNaN(parseFloat(cfg.max)) ? Number(cfg.max) : null,
        step: cfg && Number(cfg.step) ? cfg.step : 1,
        callback: cfg && cfg.callback ? cfg.callback : null
    };

    var $input = $(this);
    var min = this.sliderCfg.min;
    var max = this.sliderCfg.max;
    var step = this.sliderCfg.step;
    var callback = this.sliderCfg.callback;

    $input.attr('min', min)
        .attr('max', max)
        .attr('step', step);

    $input.bind("input", function(e){
        $input.attr('value', this.value);
        $input.css( 'background', 'linear-gradient(to right, #059CFA, white ' + this.value + '%, white)' );

        if ($.isFunction(callback)) {
            callback(this);
        }
    });
};
```

通过cfg对象来设置滑动控件的min, max, step属性。 
对控件绑定input事件，当滑块滑动时会触发该事件，此时完成对进度条的填充，这里我使用的是线性渐变linear-gradient(to right, #059CFA, white ’ + this.value + ‘%, white)这种方式，淡蓝色和白色两种颜色从左至右渐变，渐变的长度根据此时控件的value来确定。事件触发时同时调用回调函数，回调函数完成的功能可自行设计。 
当然你还可以根据自己的需求来监听其他事件，比如change事件，当value值改变时会触发，用法上很灵活。 
如何调用这个js文件里的函数来完成配置呢？很简单，首先在html文件里导入这个js文件，然后直接定义script节点，html代码如下：

```html
<!DOCTYPE html>
<html>
    <head>
    <title>demo</title>
        <script type="text/javascript" src="lib/jquery.js"></script>
        <script type="text/javascript"src="src/RangeSlider.js"></script>
        <link rel="stylesheet" href="css/slider.css" type="text/css">
    </head>

    <body>
        <div id="test">
            <input type="range" value="0">
        </div>

        <script>
            var change = function($input) {
                /*内容可自行定义*/
                console.log("123");
            }

            $('input').RangeSlider({ min: 0,   max: 100,  step: 0.1,  callback: change});

        </script>
    </body>
</html>
```

至此基于Chrome浏览器，对滑动控件的美化已全部完成。最后只剩下多浏览器的兼容问题了。

#### 3.5 实现多浏览器兼容

如果要兼容**Firefox**浏览器，只需要把上述css代码中的 ::-webkit-slider-runnable-track 替换为 **::-moz-range-track** ，就可以完成对轨道的美化了；把css代码中的 ::-webkit-slider-thumb 替换为 **::-moz-range-thumb** ，这是对滑块的样式进行改造；而如果是要填充进度条就很简单了，不需要像之前在RangeSlider.js中 **$input.css( ‘background’, ‘linear-gradient(to right, #059CFA, white ’ + this.value + ‘%, white)’ );** 这样实现填充，只需要新增如下的css代码即可：

```css
input[type=range]::-moz-range-progress {
    background: linear-gradient(to right, #059CFA, white 100%, white);
    height: 13px;    
    border-radius: 10px;
}
```

如果要想兼容**IE 9以上版本**的浏览器，对上述css代码要修改的地方稍微多了一些，下面先将针对**IE 9+**的css代码贴出来：

```css
input[type=range] {
    -webkit-appearance: none;
    width: 300px;
    border-radius: 10px;
}

input[type=range]::-ms-track {
    height: 25px;
    border-radius: 10px;
    box-shadow: 0 1px 1px #def3f8, inset 0 .125em .125em #0d1112;
    border-color: transparent; /*去除原有边框*/
    color: transparent; /*去除轨道内的竖线*/
}

input[type=range]::-ms-thumb {
    border: solid 0.125em rgba(205, 224, 230, 0.5);
    height: 25px;
    width: 25px;
    border-radius: 50%;
    background: #ffffff;
    margin-top: -5px;
    box-shadow: 0 .125em .125em #3b4547;
}

input[type=range]::-ms-fill-lower {
    /*进度条已填充的部分*/
    height: 22px;
    border-radius: 10px;
    background: linear-gradient(to right, #059CFA, white 100%, white);
}

input[type=range]::-ms-fill-upper {
    /*进度条未填充的部分*/
    height: 22px;
    border-radius: 10px;
    background: #ffffff;
}

input[type=range]:focus::-ms-fill-lower {
    background: linear-gradient(to right, #059CFA, white 100%, white);
}

input[type=range]:focus::-ms-fill-upper {
    background: #ffffff;
}
```

以上就是为了兼容IE 9+完整的css代码，也不是很复杂，同样的和Firefox浏览器一样，它支持直接使用css来自定义进度条，所以原先在RangeSlider.js里的 **$input.css( ‘background’, ‘linear-gradient(to right, #059CFA, white ’ + this.value + ‘%, white)’ );** 填充方法就不需要啦。

### 下面提几个IE浏览器需要特别注意的问题

1. 在测试时发现，IE浏览器没有加载css文件，导致样式没有发生改变，如果你的使用IE浏览器测试时也存在这样的问题，那么你需要将HTML第一行的 `<!DOCTYPE html>` 改为 `<!DOCTYPE>`；
2. 拖动滑块时，IE浏览器没有触发 **input** 事件，所以只能选择将RangeSlider.js中的监听事件改为 **change** 事件。

### 更新轨道(track)颜色填充处理方式

之前针对Chrome浏览器做的轨道颜色填充处理，效果上不是很好，因为填充的颜色是渐变的，所以靠近滑块那段的颜色会变浅；而且需要末段颜色和轨道最初颜色保持一致，用法不够灵活。现在更改一下代码：

**首先**，修改css文件中 **input[type=range]** 这部分内容，新增代码：

```css
background: -webkit-linear-gradient(#059CFA, #059CFA) no-repeat;
background-size: 0% 100%;
```

因为这里默认滑动条初始从0开始，所以数值为0%。

接下来，修改js文件： 
将这行代码 **$input.css( ‘background’, ‘linear-gradient(to right, #059CFA, white ’ + this.value + ‘%, white)’ );** 改为

```javascript
$input.css( 'background-size', this.value + '% 100%' ); 
```

稍作修改后，滑动条效果如下（靠近滑块的部分，颜色没有变浅）： 
![这里写图片描述](http://img.blog.csdn.net/20161113102448240)

若要兼容**FireFox**和**IE**则还是只需要**CSS**，不需要通过**js**来做填充处理，兼容这个效果，FireFox只需修改 **input[type=range]::-moz-range-progress**这部分；IE 9+需要修改 **input[type=range]::-ms-fill-lower** 和 **input[type=range]:focus::-ms-fill-lower** 这两部分，改法很简单，将 **backgound**的内容替换为

```css
background: linear-gradient(#059CFA, #059CFA) no-repeat;
```

### 更新关于兼容IE9+浏览器的内容

（一）针对IE浏览器可能出现css无法加载，导致我们样式没有体现出来的问题。之前的做法是将HTML第一行的 `<!DOCTYPE html>` 改为 `<!DOCTYPE>`，这种做法不是很好，后来在网上找到了更好地解决方案，详细内容可参考这个链接：[http://www.uedsc.com/css-mime-type-mismatch.html](http://www.uedsc.com/css-mime-type-mismatch.html) 

这里简单说一下本地使用浏览器时的解决方案： 
① 点击[win+R]键，在弹出的运行窗口输入“**regedit**”打开注册表编辑器； 
② 找到 **HKEY_LOCAL_MACHINE\SOFTWARE\Classes** 下的 **.css** 项，确保 **Content Type** 为 **text/css** 即可。 
![这里写图片描述](http://img.blog.csdn.net/20170121174117144?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzM0NzI0MQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

（二）在IE浏览器运行程序的时候，底部可能会提示你说已限制运行脚本，这会导致JS文件加载不了，点击“允许阻止的内容”即可。 
![这里写图片描述](http://img.blog.csdn.net/20170121173821850?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzM0NzI0MQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)



### 参考文章

[自定义(滑动条)input type=”range” 样式](http://blog.csdn.net/u013347241/article/details/51560290)

[自定义(滑动条)input type=”range” 样式下载](http://download.csdn.net/detail/u013347241/9742764)