---
title: 使用vue开发模态框组件
date: 2016-06-24 11:27:21
tags:
- vue
- 前端技术
categories:
- vue 
---
#### 示例
**本示例用到的 ```vue``` 特性有：** 组件化、prop属性传递、slot分发内容、过度、双向绑定。
**功能点有：** 信息提示、支持拖动。

> **先看下Demo效果：**

{% iframe //jsfiddle.net/kx1j6hau/3/embedded/result,js,html,css %}

<!-- more -->

-----------------------------------------------------------------------------万恶的分界线-----------------------------------------------------------------------------------
#### 画模版

> 首先我们要做的是什么呢？ 第一步就是在页面中先画一个静态的模态框模版，并给它加点样式。

html: 
{% codeblock lang:html %}
 <div class="modal-shade">
    <div class="modal-container">
      <div class="modal-wrapper">
        <div class="modal-header">
          提示
        </div>
        <div class="modal-content">
          这是一条提示信息！
        </div>
        <div class="modal-footer">
          <button class="btn btn-success">确定</button>
        </div>
      </div>
    </div>
  </div>
{% endcodeblock %}
css: 
{% codeblock lang:css %}
.modal-shade {
  position: fixed;
  z-index: 1000;
  background-color: rgba(0, 0, 0, .5);
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  transition: opacity .3s ease;
}

.modal-wrapper {
  width: 50%;
  min-width: 200px;
  height: 150px;
  background-color: #FFF;
  box-shadow: 0 2px 8px rgba(0, 0, 0, .5);
  margin: 0 auto;
  margin-top: 10%;
  font-family: 'Microsoft YaHei';
  color: #555;
  border-radius: 3px;
  position: relative;
}

.modal-wrapper .modal-header {
  border-bottom: 1px solid #EEE;
  height: 30px;
  color: #5cb85c;
  line-height: 32px;
  padding: 6px;
}

.modal-wrapper .modal-content {
  position: relative;
  padding: 15px;
}

.modal-wrapper .modal-footer {
  padding: 15px;
  text-align: right;
}

.btn {
  display: inline-block;
  background-image: none;
  border: 1px solid transparent;
  color: #FFF;
  padding: 3px 10px;
  font-family: 'Microsoft YaHei';
  cursor: pointer;
  border-radius: 3px;
}

.btn.btn-success:hover {
  background-color: #4cae4c;
}

.btn.btn-success {
  background-color: #5cb85c;
}
{% endcodeblock %}

#### 使用模版
一般，我们使用&lt;script type="x-template"&gt; 来包含模板，就像下面这样：
{% codeblock lang:html %}
<script type="x/template" id="modal-template">
    <div class="modal-shade">
        ...
    </div>
</script>
{% endcodeblock %}

#### 使用vue定义组件
##### 页面引入vue.js
{% codeblock lang:html %}
<script src= "http://cdn.bootcss.com/vue/1.0.25/vue.min.js""></script>
{% endcodeblock %}
##### 定义组件
{% codeblock lang:js %}
Vue.component('modal', {
  template: '#modal-template',
  props: {
    show: {
      type: Boolean, // 属性类型
      required: true, // 是否必填
      twoWay: true // 双向绑定
    }
  }
});
{% endcodeblock %}

组件的名字叫 ```modal``` , 组件使用的模版（template）就是我们之前定义好的模版。```#modal-template``` 表示模版的唯一标识。

#### 初始化vue
切记， 一定要在初始化vue之前定义好组件。
{% codeblock lang:js %}
new Vue({
  el: 'body',
  data: {
    showModal: false // 是否显示模态框，默认不显示。
  }
});
{% endcodeblock %}

#### 使用组件 
执行以上步骤后，我们就可以在页面中使用组件了：
{% codeblock lang:html %}
<button class="btn btn-success" @click="showModal = true">show modal</button>
<modal :show.sync="showModal"></modal>
{% endcodeblock %}
这里有两个地方需要注意：
1. **@click="showModal = true"**: 绑定事件监听事件。当点击按钮的时候，设置 ```showModal = true``` , 这里的 @click 是vue中的简写形式，也可以写成 v-on:click，更多事件绑定请阅读： [API](http://cn.vuejs.org/api/#v-on)
2. **:show.sync="showModal"**： 动态绑定Props值。```.sync``` 双向绑定，强制把 ```showModal``` 属性的值绑定到```show``` 属性中。

#### 使用 ```v-if``` 进行条件渲染
你会发现，当你刷新页面的时候，模态框自己出来了。

**为什么会自己弹出来？**
因为我们在页面引用了我们之前定义的 <code>&lt;modal/&gt;</code> 组件。 但是，这本不是我们想要的。我们希望当点击页面某个按钮的时候，模态框才出来。

我们可以在模版中添加 ```v-if``` 属性，对 ```show``` 进行判断，如果是true则显示，相反则不显示。来，让我们试试，代码如下：
{% codeblock lang:html %}
...
<div class="modal-shade" v-if="show" transition="modal">
    ...
</div>
...
{% endcodeblock %}

这次是不是显示了？ 试试点下按钮（show modal）。我的天呐，这么神奇吗？ 模态框居然弹出框了，而且还带遮罩！

#### 给模态框来点过度效果
为了应用过渡效果，需要在目标元素上使用 transition 特性：
{% codeblock lang:html %}
<div class="modal-shade" v-if="show" transition="modal"></div>
{% endcodeblock %}

添加css过度效果：
{% codeblock lang:css %}
.modal-container {
  transition: all .3s ease;
}

.modal-enter,
.modal-leave {
  opacity: 0;
}

.modal-enter .modal-container,
.modal-leave .modal-container {
  transform: translate(0px, -30px);
}
{% endcodeblock %}

更多具体细节，请阅读[Vue教程](http://cn.vuejs.org/guide/transitions.html)

#### 使用 ```slot``` 分发内容
假如我们有个需求，模态框的内容部分可以动态改变，也就是说，用户可以自定义提示内容，提示内容代码是这一部分：
{% codeblock lang:html %}
<div class="modal-content">
  这是一条提示信息！
</div>
{% endcodeblock %}

很简单，我们可以这样做，用 <code>&lt;slot/&gt;</code> 标签包裹内容信息。上代码：
{% codeblock lang:html %}
<div class="modal-content">
  <slot name="content">这是一条提示信息！</slot>
</div>
{% endcodeblock %}

并且，给 ```slot``` 起一个名字。

然后，在组件中自定义我们的内容：
{% codeblock lang:html %}
<modal :show.sync="showModal">
  <span slot="content">哈哈，内容被覆盖了！</span>
</modal>
{% endcodeblock %}

是不是被覆盖了！关于slot的更多用法，请阅读[Vue教程](http://cn.vuejs.org/guide/components.html#使用-Slot-分发内容)

好了，目前为止，我们的组件已经开发完了。额... 因为我是处女座，你懂得！ 如果模态框能够拖动就好了！

于是，我在豆浆机里搅了20分钟后... 

#### 让模态框拖动起来

下面的代码依赖 ```jquery``` 没有引入的同学请自觉依赖~

好了，我把能拖动的代码附在下面，每一步的注释，我都标注的很清楚，这里就不一一说明了，看代码：
{% codeblock lang:js %}
var mouseStart = {}, //记录鼠标开始坐标
  dialogStart = {}, //弹出框开始坐标
  $this;

// 绑定鼠标按下事件
$(document).delegate('.modal-header', 'mousedown', function(event) {
  //改变鼠标为拖拽图标
  $this = $(this).css('cursor', 'move');

  //设置弹出框透明度
  $this.parent().fadeTo('fast', 0.8);

  mouseStart.x = event.clientX; //鼠标x轴坐标
  mouseStart.y = event.clientY; //鼠标y轴坐标
  dialogStart.x = $this.parent().offset().left; //弹出框距离浏览器左侧距离
  dialogStart.y = $this.parent().offset().top; // 弹出框距离浏览器顶部距离

  //绑定鼠标移动事件
  $(document).bind('mousemove', Mousemove).bind('mouseup', Mouseup);
});

//鼠标移动事件
function Mousemove(event) {
  var moveX = event.clientX - mouseStart.x, //获取鼠标x轴移动的距离 
    moveY = event.clientY - mouseStart.y; //获取鼠标y轴移动的距离

  var targetLeft = parseInt($this.parent().css('marginLeft'), 10), //获取弹出框的margin-left
    targetTop = parseInt($this.parent().css('marginTop'), 10), //获取弹出框的margin-top
    targetWidth = $this.parent().width(), //弹出框宽度
    targetHeight = $this.parent().height(), //弹出框高度
    newWidth = moveX + dialogStart.x - targetLeft, //新的宽度
    newHeight = moveY + dialogStart.y - targetTop; //新的高度


  // 计算弹出框左右拖动不能超出可视区域
  if ((newWidth + targetLeft) < 0) {
    newWidth = -targetLeft;
  } else if (newWidth > (document.documentElement.clientWidth - targetWidth - targetLeft)) {
    newWidth = (document.documentElement.clientWidth - targetWidth - targetLeft);
  }

  // 计算弹出框上下拖动不能超出可视区域
  if ((newHeight + targetTop) < 0) {
    newHeight = -targetTop;
  } else if (newHeight > (document.documentElement.clientHeight - targetHeight - targetTop)) {
    newHeight = (document.documentElement.clientHeight - targetHeight - targetTop);
  }

  // 设置弹出框新的宽高
  $this.parent().css({
    left: newWidth + 'px',
    top: newHeight + 'px'
  });

}

//鼠标放开事件
function Mouseup() {
  $this.css('cursor', 'default');
  $this.parent().fadeTo('fast', 1);

  //释放绑定事件
  $(document).unbind('mousemove', Mousemove).unbind('mouseup', Mouseup);
}
{% endcodeblock %}


enjoy it ~

我是一枚喜欢玩前端的后端攻城狮，技术有限，代码如有错误之处，还请大神劈正！




