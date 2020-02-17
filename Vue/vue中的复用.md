# Vue 中组件的复用

一看到组件的复用，就想起封装组件。那么我们一般只会想到 component，但是其实在 Vue.js 里，还有几个 API 可以起到复用的效果。

你知道 extend,mixins,extends，components,install 用法吗? 若不知道，下面都能找到这些答案。

## component

Vue.component 是注册全局组件,为了方便。

当模板中遇到该组件名称作为标签的自定义元素时，会自动调用类似于 new Vue()的函数构造器来生成组件实例，并挂载到自定义元素上，当然实际情况要比这复杂得多，还需要处理 props 数据传递、slot 内容分发、自定义事件、组件生命周期等。

```javascript
Vue.component("component-name", { ...(component - config) });
```

等同于

```javascript
Vue.component("component-name", { ...(component - config) });
```

在使用 Vue.component 时候，其会自动判断第二个传进来的是 Vue 继承对象（Vue.extend）还是普通对象({...})。如果传进来的是普通对象的话会自动调用 Vue.extend。

### component 的实际应用

1. 注册全局组件

```javascript
import Button from "@/components/Button";
Vue.component("wbButton", Button);
```

## extend

Vue.extend 返回的是一个**扩展实例构造器**，也就是预设了部分选项的 Vue 实例构造器。其主要用来服务于 Vue.component。

> extend 创建的是一个组件构造器，而不是一个具体的组件实例。所以不能直接在 new Vue 中这样使用： new Vue({components: button})。最终还是要通过 Vue.components 注册才可以使用的。

```javascript
// 创建构造器
var Profile = Vue.extend({
  template: "<p>{{extendData}}</br>实例传入的数据为:{{propsExtend}}</p>",
  data: function() {
    return {
      extendData: "这是extend扩展的数据"
    };
  },
  props: ["propsExtend"]
});
// 创建 Profile 实例，并挂载到一个元素上。
new Profile({
  propsData: { propsExtend: "我是实例传入的数据" }
}).$mount("#app-extend");
```

### extend 的实际应用

1. 弹窗，通知

一般使用 UI 库，使用通知组件会这样：

```javascript
mounted() {
    this.$notify('提示文案');
  }
```

实现的原理一般分为两部分： **JavaScript 和 vue**

alert.js

```javascript
import Vue from "vue";

const ToastConstructor = Vue.extend(require("./toast.vue"));
let toastPool = [];

let getAnInstance = () => {
  if (toastPool.length > 0) {
    let instance = toastPool[0];
    toastPool.splice(0, 1);
    return instance;
  }
  return new ToastConstructor({
    el: document.createElement("div")
  });
};

let returnAnInstance = instance => {
  if (instance) {
    toastPool.push(instance);
  }
};

let removeDom = event => {
  if (event.target.parentNode) {
    event.target.parentNode.removeChild(event.target);
  }
};

ToastConstructor.prototype.close = function() {
  this.visible = false;
  this.$el.addEventListener("transitionend", removeDom);
  this.closed = true;
  returnAnInstance(this);
};

let Toast = (options = {}) => {
  let duration = options.duration || 3000;

  let instance = getAnInstance();
  instance.closed = false;
  clearTimeout(instance.timer);
  instance.message = typeof options === "string" ? options : options.message;

  document.body.appendChild(instance.$el);
  Vue.nextTick(function() {
    instance.visible = true;
    instance.$el.removeEventListener("transitionend", removeDom);
    ~duration &&
      (instance.timer = setTimeout(function() {
        if (instance.closed) return;
        instance.close();
      }, duration));
  });
  return instance;
};

export default Toast;
```

alert.vue

```html
<template>
  <transition name="toast-pop">
    <div class="toast" v-show="visible">
      <span>{{ message }}</span>
    </div>
  </transition>
</template>
<script type="text/babel">
  export default {
    props: {
      message: {
        type: String
      }
    },
    data() {
      return {
        visible: false
      };
    }
  };
</script>
```

## mixins

mixins 是对组件的扩充，包括 methods、components、directive 等。它接收对象数组（可理解为多继承）

### 规则

1. 触发生命周期钩子函数时，先触发 mixins 组件中的钩子，再调用组件自身的函数。
2.

## extends

## install
