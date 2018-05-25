title: vue3.0快速创建项目&配置
author: 徐子玉
tags:
  - vue
categories:
  - vue
date: 2018-05-25 12:51:00
---
## 本地安装vue-cli
---------------------------------------
### 前置条件
* 更新npm到最新版本  
> 命令行运行:  
`npm install -g npm`  
npm就自动为我们更新到最新版本  
* 淘宝npm镜像使用方法
> `npm config set registry https://registry.npm.taobao.org`  
`cnpm config set registry https://registry.npm.taobao.org`  
`yarn config set registry https://registry.npm.taobao.org`  
配置后可通过下面方式来验证是否成功 ：  
    `npm config get registry`
* 使用 npm 全局安装 vue-cli ：  
`npm i -g @vue/cli@3.0.0-beta.6`  

<!-- more -->

## 创建项目
--------------
1. 执行：  
`vue create my-project`  
2. 此处有两个选择：  
	* `default (babel, eslint)`默认套餐，提供`babel`和`eslint`支持  
	*  `Manually select features`自己去选择需要的功能，提供更多的特性选择。比如如果想要支持 TypeScript ，就应该选择这一项。
	* 可以使用`上下方向键`来切换选项。如果只需要 `babel` 和 `eslint` 支持，那么选择第一项，就完事了，静静等待 vue 初始化项目。
	*	vue-cli 内置支持了8个功能特性，可以多选：使用方向键在特性选项之间切换，使用空格键选中当前特性，使用 a 键切换选择所有，使用 i 键翻转选项。
	* 对于每一项的功能，此处做个简单描述：
   *	`TypeScript` 支持使用 `TypeScript` 书写源码
   *	`Progressive Web App (PWA) Support` PWA 支持。
   *	`Router` 支持 vue-router 。
   *	`Vuex` 支持 vuex 。
   *	`CSS Pre-processors` 支持 CSS 预处理器。
   *	`Linter / Formatter` 支持代码风格检查和格式化。
   *	`Unit Testing` 支持单元测试。
   *	`E2E Testing` 支持 E2E 测试。  
	*	我选择了 `Router`，`Vuex`，`CSS Pre-processors`，`Linter / Formatter`  
3. 按住enter进入下一步，接下来都是对之前每项选项的更详细的选择。  
	*	**css**选择`SCSS/SASS`
	*	**Linter / Formatter**选择`prettier`  
	*	这一步就是要选择配置文件的位置了。对于 `Babel` 、 `PostCSS` 等，都可以有自己的配置文件： `.babelrc` 、 `.postcssrc` 等等，同时也可以把配置信息放在 `package.json` 里面。此处出于对编辑器（ Visual Studio Code ）的友好支持（编辑器一般默认会在项目根目录下寻找配置文件），选择把配置文件放在外面，选择 `In dedicated config files`
	*	待补充  
4.	`Save this as a preset for future projects?`这个就是问要不要把当前的这一系列选项配置保存起来，方便下一次创建项目时复用。选择`y`。
5.	选完之后， vue-cli 就根据前面选择的内容，开始初始化项目了。

## 启动项目
-------------
初始完之后，进入到项目根目录：  
`cd my-project`  
启动项目：  
`npm run serve`   
稍等一会儿，可以看到自动在浏览器中打开了  
#### 安装PostCSS插件  
1.	通过`Vue-cli`构建的项目，在项目的根目录下有一个`.postcssrc.js`，默认情况下已经有了：
```
module.exports = {
        plugins: {
          autoprefixer: {}
        }
}
``` 
配置成
```
module.exports = {
        plugins: {
          "postcss-import":{},
          "postcss-url":{},
          "autoprefixer": {}
        }
}
```
2.	安装`postcss-import`和`postcss-url`插件  
	*	`$ npm install postcss-import`和`$ npm install postcss-url`
	*	`postcss-import`相关配置[*点击这里*](http://github.com/postcss/postcss-import "postcss-import的github仓库")。主要功有是解决`@import`引入路径问题。使用这个插件，可以让你很轻易的使用本地文件、`node_modules`或者`web_modules`的文件。这个插件配合`postcss-url`让你引入文件变得更轻松。  
    *	`postcss-url`相关配置可以[*点击这里*](https://github.com/postcss/postcss-url "postcss-url的github仓库")。该插件主要用来处理文件，比如图片文件、字体文件等引用路径的处理。在Vue项目中，`vue-loader`已具有类似的功能，只需要配置中将`vue-loader`配置进去。  
    *	`autoprefixer`插件是用来自动处理浏览器前缀的一个插件。如果你配置了`postcss-cssnext`，其中就已具备了`autoprefixer`的功能。在配置的时候，未显示的配置相关参数的话，表示使用的是[`Browserslist`](https://github.com/ai/browserslist "Browserslist的github仓库地址")指定的列表参数，你也可以像这样来指定`last 2 versions` 或者 `> 5%`。如此一来，你在编码时不再需要考虑任何浏览器前缀的问题，可以专心撸码。这也是PostCSS最常用的一个插件之一。  
    
###	其他插件  
我们要完成vw的布局兼容方案，或者说让我们能更专心的撸码，还需要配置下面的几个PostCSS插件：
*	[postcss-aspect-ratio-mini](http://github.com/yisibl/postcss-aspect-ratio-mini)
*	[postcss-px-to-viewport](http://github.com/evrone/postcss-px-to-viewport)
*	[postcss-write-svg](http://github.com/jonathantneal/postcss-write-svg)
*	[postcss-cssnext](http://github.com/MoOx/postcss-cssnext)
*	[cssnano](http://github.com/ben-eb/cssnano)
*	[postcss-viewport-units](http://github.com/springuper/postcss-viewport-units)  

要使用  
安装成功后，在项目根目录下的`package.json`文件中，可以看到新安装的依赖包：
```javascript
"dependencies": {
    "cssnano": "^3.10.0",
    "postcss-aspect-ratio-mini": "^0.0.2",
    "postcss-cssnext": "^3.1.0",
    "postcss-import": "^11.1.0",
    "postcss-px-to-viewport": "^0.0.3",
    "postcss-url": "^7.3.2",
    "postcss-viewport-units": "^0.1.4",
    "postcss-write-svg": "^3.0.1",
    "vue": "^2.5.16",
    "vue-router": "^3.0.1",
    "vuex": "^3.0.1"
  },
```
接下来在`.postcssrc.js`文件对新安装的`PostCSS`插件进行配置：
```
module.exports = {
  plugins: {
    "postcss-import": {},
    "postcss-url": {},
    //"autoprefixer": {},
    "postcss-aspect-ratio-mini": {},
    "postcss-write-svg": {
      utf8: false
    },
    "postcss-cssnext": {},
    "postcss-px-to-viewport": {
      viewportWidth: 750, //视窗的宽度，对应的是我们设计稿的宽度，一般是750
      viewportHeight: 1334, //视窗的高度，根据750设备的宽度来置顶，一般指定1334，也可以不配置
      unitPrecision: 3, //指定'px'转换为视窗单位值的小数位数（很多时候无法整除）
      viewportUnit: 'vw', //指定需要转换成的视窗单位，建议使用vw
      selectorBlackList: ['.ignore', '.hairlines', '.g-vw-no'], //指定不转行为视窗单位的类，可以自定义，可以无限添加，建议定义一至两个通用的类名
      minPixeValue: 1, //小于或等于'1px'不转换为视窗单位，你也可以设置为你想要的值
      mediaQuery: false //允许在媒体查询中转换'px'
    },
    "postcss-viewport-units": {},
    "cssnano": {
      preset: "advanced",
      autoprefixer: false,
      "postcss-zindex": false
    }
  }
}
```

> 特别声明：由于`cssnext`和`cssnano`都具有`autoprefixer`,事实上只需要一个，所以把默认的`autoprefixer`删除掉，然后把`cssnano`中的`autoprefixer`设置为`false`。对于其他的插件使用，稍后会简单的介绍。

由于配置文件修改了，所以重新跑一下`npm run dev`。项目就可以正常看到了。接下来简单的介绍一下后面安装的几个插件的作用。
##### postcss-cssnext

`postcss-cssnext`其实就是`cssnext`。该插件可以让我们使用CSS未来的特性，其会对这些特性做相关的兼容性处理。其包含的特性主要有：  
有关于cssnext的每个特性的操作文档，可以[点击这里](http://cssnano.co/guides/getting-started/)浏览。
##### cssnano

`cssnano`主要用来压缩和清理`CSS`代码。在`Webpack`中，`cssnano`和`css-loader`捆绑在一起，所以不需要自己加载它。不过你也可以使用`postcss-loader`显式的使用`cssnano`。有关于`cssnano`的详细文档，可以点击这里获取。

在`cssnano`的配置中，使用了`preset: "advanced"`，所以我们需要另外安装：
```
npm i cssnano-preset-advanced --save-dev
```
`cssnano`集成了一些其他的`PostCSS`插件，如果你想禁用`cssnano`中的某个插件的时候，可以像下面这样操作：
```
"cssnano": {
    autoprefixer: false,
    "postcss-zindex": false
}
```
上面的代码把`autoprefixer`和`postcss-zindex`禁掉了。前者是有重复调用，后者是一个讨厌的东东。只要启用了这个插件，`z-index`的值就会重置为1。这是一个天坑，***千万记得将`postcss-zindex`设置为`false`***。
postcss-px-to-viewport

postcss-px-to-viewport插件主要用来把px单位转换为vw、vh、vmin或者vmax这样的视窗单位，也是vw适配方案的核心插件之一。

在配置中需要配置相关的几个关键参数：
```javascript
"postcss-px-to-viewport": {
    viewportWidth: 750,      // 视窗的宽度，对应的是我们设计稿的宽度，一般是750
    viewportHeight: 1334,    // 视窗的高度，根据750设备的宽度来指定，一般指定1334，也可以不配置
    unitPrecision: 3,        // 指定`px`转换为视窗单位值的小数位数（很多时候无法整除）
    viewportUnit: 'vw',      // 指定需要转换成的视窗单位，建议使用vw
    selectorBlackList: ['.ignore', '.hairlines'],  // 指定不转换为视窗单位的类，可以自定义，可以无限添加,建议定义一至两个通用的类名
    minPixelValue: 1,       // 小于或等于`1px`不转换为视窗单位，你也可以设置为你想要的值
    mediaQuery: false       // 允许在媒体查询中转换`px`
}
```
目前出视觉设计稿，我们都是使用`750px`宽度的，那么`100vw = 750px`，即`1vw = 7.5px`。那么我们可以根据设计图上的`px`值直接转换成对应的`vw`值。在实际撸码过程，不需要进行任何的计算，直接在代码中写`px`，比如：
```
.test {
    border: .5px solid black;
    border-bottom-width: 4px;
    font-size: 14px;
    line-height: 20px;
    position: relative;
}
[w-188-246] {
    width: 188px;
}

编译出来的CSS：

.test {
    border: .5px solid #000;
    border-bottom-width: .533vw;
    font-size: 1.867vw;
    line-height: 2.667vw;
    position: relative;
}
[w-188-246] {
    width: 25.067vw;
}
```
在不想要把`px`转换为`vw`的时候，首先在对应的元素（`html`）中添加配置中指定的类名`.ignore`或`.hairlines`(`.hairlines`一般用于设置`border-width:0.5px`的元素中)：
```
<div class="box ignore"></div>
```
写CSS的时候：
```
.ignore {
    margin: 10px;
    background-color: red;
}
.box {
    width: 180px;
    height: 300px;
}
.hairlines {
    border-bottom: 0.5px solid red;
}
```
编译出来的CSS:
```
.box {
    width: 24vw;
    height: 40vw;
}
.ignore {
    margin: 10px; /*.box元素中带有.ignore类名，在这个类名写的`px`不会被转换*/
    background-color: red;
}
.hairlines {
    border-bottom: 0.5px solid red;
}
```
上面解决了`px`到`vw`的转换计算。那么在哪些地方可以使用`vw`来适配我们的页面。根据相关的测试：
* 容器适配，可以使用vw
* 文本的适配，可以使用vw
* 大于1px的边框、圆角、阴影都可以使用vw
* 内距和外距，可以使用vw

##### postcss-aspect-ratio-mini

[`postcss-aspect-ratio-mini`](http://github.com/yisibl/postcss-aspect-ratio-mini)主要用来处理元素容器宽高比。在实际使用的时候，具有一个默认的结构
```
<div aspectratio>
    <div aspectratio-content></div>
</div>
```
在实际使用的时候，你可以把自定义属性`aspectratio`和`aspectratio-content`换成相应的类名，比如：
```
<div class="aspectratio">
    <div class="aspectratio-content"></div>
</div>
```
我个人比较喜欢用__自定义属性__，它和类名所起的作用是同等的。结构定义之后，需要在你的样式文件中添加一个**统一的宽度比默认属性**：
```
[aspectratio] {
    position: relative;
}
[aspectratio]::before {
    content: '';
    display: block;
    width: 1px;
    margin-left: -1px;
    height: 0;
}
/*aspectratio-content存放内容，自由填满，利用伪元素::before和::after来撑开容器*/
[aspectratio-content] {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    width: 100%;
    height: 100%;
}
```
如果我们想要做一个`188:246`（`188`是容器宽度，`246`是容器高度）这样的比例容器，只需要这样使用：
```
[w-188-246] {
    aspect-ratio: '188:246';
}
```
有一点需要特别注意：`aspect-ratio`属性不能和其他属性写在一起，否则编译出来的属性只会留下`aspect-ratio`的值，比如：
```
<div aspectratio w-188-246 class="color"></div>
```
编译前的CSS如下：
```
[w-188-246] {
    width: 188px;
    background-color: red;
    aspect-ratio: '188:246';
}
```
编译之后：
```
[w-188-246]:before {
    padding-top: 130.85106382978725%;
}
```
主要是因为在插件中做了相应的处理，不在每次调用`aspect-ratio`时，生成前面指定的默认样式代码，这样代码没那么冗余。所以在使用的时候，需要把`width`和`background-color`分开来写：
```
[w-188-246] {
    width: 188px;
    background-color: red;
}
[w-188-246] {
    aspect-ratio: '188:246';
}
```
这个时候，编译出来的CSS就正常了：
```
[w-188-246] {
    width: 25.067vw;/*撑开宽度*/
    background-color: red;
}
[w-188-246]:before {
    padding-top: 130.85106382978725%;/*撑开高度*/
}
```
有关于宽高比相关的详细介绍，如果大家感兴趣的话，可以阅读下面相关的文章：

* [CSS实现长宽比的几种方案](http://www.w3cplus.com/css/aspect-ratio.html)
* [容器长宽比](http://www.w3cplus.com/css/aspect-ratio-boxes.html)
* [Web中如何实现纵横比](http://www.w3cplus.com/css/experiments-in-fixed-aspect-ratios.html)
* [实现精准的流体排版原理](http://www.w3cplus.com/css/css-polyfluidsizing-using-calc-vw-breakpoints-and-linear-equations.html)

> 目前采用`PostCSS`插件只是一个过渡阶段，在将来我们可以直接在CSS中使用`aspect-ratio`属性来实现长宽比。
##### postcss-write-svg

[`postcss-write-svg`](http://github.com/jonathantneal/postcss-write-svg)插件主要用来处理移动端`1px`的解决方案。该插件主要使用的是`border-image`和`background`来做`1px`的相关处理。比如：
```
@svg 1px-border {
    height: 2px;
    @rect {
        fill: var(--color, black);
        width: 100%;
        height: 50%;
    }
}
.example {
    border: 1px solid transparent;
    border-image: svg(1px-border param(--color #00b1ff)) 2 2 stretch;
}
```
编译出来的CSS:
```
.example {
    border: 1px solid transparent;
    border-image: url("data:image/svg+xml;charset=utf-8,%3Csvg xmlns='http://www.w3.org/2000/svg' height='2px'%3E%3Crect fill='%2300b1ff' width='100%25' height='50%25'/%3E%3C/svg%3E") 2 2 stretch;
}
```
上面演示的是使用`border-image`方式，除此之外还可以使用`background-image`来实现。比如：
```
@svg square {
    @rect {
        fill: var(--color, black);
        width: 100%;
        height: 100%;
    }
}

#example {
    background: white svg(square param(--color #00b1ff));
}
```
编译出来就是：
```
#example {
    background: white url("data:image/svg+xml;charset=utf-8,%3Csvg xmlns='http://www.w3.org/2000/svg'%3E%3Crect fill='%2300b1ff' width='100%25' height='100%25'/%3E%3C/svg%3E");
}
```
解决`1px`的方案除了这个插件之外，还有其他的方法。可以阅读前期整理的《[再谈Retina下`1px`的解决方案](http://www.w3cplus.com/css/fix-1px-for-retina.html)》一文。

> **特别声明**：由于有一些低端机对`border-image`支持度不够友好，个人建议你使用`background-image`的这个方案。
##### CSS Modules

Vue中的`vue-loader`已经集成了[CSS Modules](http://github.com/css-modules/css-modules)的功能，个人建议在项目中开始使用CSS Modules。特别是在Vue和React的项目中，CSS Modules具有很强的优势和灵活性。建议看看CSS In JS相关的资料。在Vue中，使用CSS Modules的相关文档可以阅读Vue官方提供的文档[《CSS Modules》](http://vue-loader.vuejs.org/en/features/css-modules.html)。
##### postcss-viewport-units

[`postcss-viewport-units`](http://github.com/springuper/postcss-viewport-units)插件主要是给CSS的属性添加`content`的属性，配合[`viewport-units-buggyfill`](http://github.com/rodneyrehm/viewport-units-buggyfill)库给`vw`、`vh`、`vmin`和`vmax`做适配的操作。

这是实现`vw`布局必不可少的一个插件，因为少了这个插件，这将是一件痛苦的事情。后面你就清楚。

到此为止，有关于所需要的PostCSS已配置完。并且简单的介绍了各个插件的作用，至于详细的文档和使用，可以参阅对应插件的官方文档。
### vw兼容方案
使用`viewport`的`polyfill`：`Viewport Units Buggyfill`。使用`viewport-units-buggyfill`主要分以下几步走：
##### 引入JavaScript文件

`viewport-units-buggyfill`主要有两个JavaScript文件：`viewport-units-buggyfill.js`和`viewport-units-buggyfill.hacks.js`。你只需要在你的HTML文件中引入这两个文件。比如在Vue项目中的`index.html`引入它们：
```
<script src="//g.alicdn.com/fdilab/lib3rd/viewport-units-buggyfill/0.6.2/??viewport-units-buggyfill.hacks.min.js,viewport-units-buggyfill.min.js"></script>
```
你也可以使用其他的在线CDN地址，也可将这两个文件合并压缩成一个`.js`文件。这主要看你自己的兴趣了。

第二步，在HTML文件中调用`viewport-units-buggyfill`，比如：
```
<script>
    window.onload = function () {
        window.viewportUnitsBuggyfill.init({
            hacks: window.viewportUnitsBuggyfillHacks
        });
    }
</script>
```
为了你Demo的时候能获取对应机型相关的参数，我在示例中添加了一段额外的代码，估计会让你有点烦：
```
<script>
    window.onload = function () {
        window.viewportUnitsBuggyfill.init({
        hacks: window.viewportUnitsBuggyfillHacks
        });

        var winDPI = window.devicePixelRatio;
        var uAgent = window.navigator.userAgent;
        var screenHeight = window.screen.height;
        var screenWidth = window.screen.width;
        var winWidth = window.innerWidth;
        var winHeight = window.innerHeight;

        alert(
            "Windows DPI:" + winDPI +
            ";\ruAgent:" + uAgent +
            ";\rScreen Width:" + screenWidth +
            ";\rScreen Height:" + screenHeight +
            ";\rWindow Width:" + winWidth +
            ";\rWindow Height:" + winHeight
        )
    }
</script>
```
具体的使用。在你的CSS中，只要使用到了`viewport`的单位（`vw`、`vh`、`vmin`或`vmax` ）地方，需要在样式中添加`content`：
```
.my-viewport-units-using-thingie {
    width: 50vmin;
    height: 50vmax;
    top: calc(50vh - 100px);
    left: calc(50vw - 100px);

    /* hack to engage viewport-units-buggyfill */
    content: 'viewport-units-buggyfill; width: 50vmin; height: 50vmax; top: calc(50vh - 100px); left: calc(50vw - 100px);';
}
```
这可能会令你感到恶心，而且我们不可能每次写vw都去人肉的计算。特别是在我们的这个场景中，咱们使用了`postcss-px-to-viewport`这个插件来转换`vw`，更无法让我们人肉的去添加`content`内容。

这个时候就需要前面提到的`postcss-viewport-units`插件。这个插件将让你无需关注`content`的内容，插件会自动帮你处理。  

[**Viewport Units Buggyfill**](http://github.com/rodneyrehm/viewport-units-buggyfill)还提供了其他的功能。详细的这里不阐述了。但是`content`也会引起一定的副作用。比如`img`和伪元素`::before`(`:before`)或`::after`（`:after`）。在`img`中`content`会引起部分浏览器下，图片不会显示。这个时候需要全局添加：
```
img {
    content: normal !important;
}
```
而对于`::after`之类的，就算是里面使用了`vw`单位，Viewport Units Buggyfill对其并不会起作用。比如：
```
// 编译前
.after {
    content: 'after content';
    display: block;
    width: 100px;
    height: 20px;
    background: green;
}

// 编译后
.after[data-v-469af010] {
    content: "after content";
    display: block;
    width: 13.333vw;
    height: 2.667vw;
    background: green;
}
```
这个时候我们需要通过添加**额外的标签**来替代伪元素（这个情景我没有测试到，后面自己亲测一下）。

到了这个时候，你就不需要再担心兼容问题了。
整个示例的源码，可以点击[这里下载](http://www.w3cplus.com/sites/default/files/blogs/2018/1801/vw-layout.zip "示例源码")。

> 如果你下载了示例源码，先要确认你的系统环境能跑Vue的项目，然后下载下来之后，解压缩，接着运行`npm i`，再运行`npm run dev`，你就可以看到效果了。


## 打包上线  
---------------------------
在开发完项目之后，就应该打包上线了。 vue-cli 也提供了打包的命令，在项目根目录下执行：  
`npm run build`  
执行完之后，可以看到在项目根目录下多出了一个 dist 目录，该目录下就是打包好的所有静态资源，直接部署到静态资源服务器就好了。  
实际上，在部署的时候要注意，假设静态服务器的域名是 `http://static.baidu.com` ，那么对应到访问 `<项目根目录>/dist/index.html` 的 URL 一定要是 `http://static.baidu.com/index.html` ，其他的静态资源以此类推。