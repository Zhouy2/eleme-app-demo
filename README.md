**v 0.1.1**
* 参照商品页设计图，增加goods.vue组件（商品），包含
    * 菜单列表
        * 菜单栏（menu）
        * 食物栏（foods）
    * 食物列表
    * 购物车

>由于图标多次使用到，可以做为一个公共组件使用

---

**v 0.1.0**

* 参照商品页设计图，给vheader.vue组件（头部）添加相应的样式

* 添加详情弹出页，并赋予样式  
- 详情弹出页使用到了[sticky-footer]的布局样式  
可以参考文章：<https://www.w3cplus.com/css3/css-secrets/sticky-footers.html>
- 使用到了[flex]布局样式  
可以参考文章：<http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html?utm_source=fuicool>  
兼容性问题：Postcss是根据can i use（<http://caniuse.com>）官网去写代码  
- 添加动画效果，使用vue的transition标签  
使用时需要注意vue2.X与vue1.X的区别，transition标签的使用完全不同。  
可以参考文章：<https://vuefe.cn/v2/guide/transitions.html>或者<https://cn.vuejs.org/v2/guide/transitions.html>  

* 增加星组件（star.vue)作为共用组件：
    * 详情弹层页
    * 评价页
    * 商家页
>**注意：**使用时，需要再star外面嵌套一层，用来定义自己的样式

####如何消除两个span标签之间的间隙？
```
有两种方法消除间隙
1、通过给span标签的父元素设置font-size:0实现
2、将两个span标签连在一起（<span>a</span><span>b</span>），不要用换行分开。
```

####关于position定位问题
```
元素使用absolute（绝对）定位时，其父元素需要使用relative（相对）来定位。
```

####如何实现背景图
```
将背景图放入到img标签中，设置其宽、高分别为100%，对该元素做绝对定位，top、left均为0，
设置z-index:-1，filter: blur(10px)
```

---

**v 0.0.3**

* 模拟后台数据，在webpack-dev-conf.js文件中配置
具体配置信息可以查看博客：<https://www.cnblogs.com/myRain/p/7904651.html>

* 引入axios来发起请求，获取模拟后台数据。
[@axios中文说明]<https://www.kancloud.cn/yunye/axios/234845>

#### 父子组件传值
```
本项目中：父组件（APP.vue）、子组件（vheader.vue）
父子组件的关系可以总结为 props down，events up。
父组件(v-bind)通过props向下传递数据给子组件(props)，子组件（$emit）通过events给父组件($on)发送消息。
可以参考文章：<https://www.cnblogs.com/xiaohuochai/p/7388866.html>
```

---

**V 0.0.2**

* 增加reset.css文件（static/reset.css），清除浏览器的一些默认样式

* 商品界面组件拆分：
    * 头部页：vheader.vue
    * 列表页：goods.vue, ratings.vue, seller.vue
	* 商品页：

* 增加stylus和stylus-loader模块，预编译CSS样式

```python
npm install stylus –save-dev 
npm install stylus-loader –save-dev
```

* 实用flex实现列表页的布局

* **vue-router官网地址**：<https://router.vuejs.org/zh-cn>

* 在src/router/index.js中配置vue-router路径地址

```javascript
<router-link to="/"></router-link>
<router-view></router-view>
```

####如何生成1px的border？

```javascript
由于手机的dpr（device-pixel-ratio）的影响，实际设置*border-bottom:1px solid black*的值，在手机上则会显示为2px。
要解决这个问题，可以通过设置一个after的伪类的border-bottom为1px，以及控制min-device-pixel-ratio的Y轴缩放来实现实际设备border的1px功能。
**dpr的简单介绍**：<http://www.zhangxinxu.com/wordpress/2012/08/window-devicepixelratio/>
```

---

**V 0.0.1**

*This is eleme app demo*  

####准备工作：

```python
1. 准备项目所需要的资源（图片）;  
2. 制作图标字体文件  
    将svg文件制作位图标字体文件，通过icomoon工具转换（<https://icomoon.io>）
3. 设计目录结构  
    root目录（项目根目录）增加resource目录（存放静态资源文件）  
    src目录增加common目录（存放共用的资源文件，包括fonts、js、stylus（css预编译文件））  
    components目录增加header目录（头部vue相关文件，根据就近原则存放相应文件）
4. 增加data.json文件，mock数据（模拟后台数据）  
    vue 1.X采用vue-resource来进行调用，vue 2.X采用axios来调用数据
```

