# flutter 布局中的原理：widget,element,renderObject

flutter 一切都为组件，这一点和 vue.js 很相似。

## widget 树

了解 HTML 的开发者肯定听过 DOM 树的概念，它由页面中每一个控件组成，这些控件所形成的一种嵌套关系使其可以表示为 “树” 结构。

我们也可以将这个概念应用在 Flutter 中，这样的布局我们成为“widget 树”，也可以叫做控件树，它就表示了我们在 dart 代码中所写的控件的结构。

比如：默认 demo 的计数器程序:

![计时器](https://cdn.jsdelivr.net/gh/flutterchina/flutter-in-action/docs/imgs/2-1.png)

他的结构树为：
![结构树](https://meandni.com/2019/05/05/flutter-principle/counterAppwidgertree.jpg)

可以看出 widget 之间是存在着“父子”关系，例如在上图中，我们可以说 Text 组件是 Column 组件的子组件，Scaffold 是 AppBar 的父组件。

## element 树

回想一下 flutter 的架构图:

![架构图](https://meandni.com/2019/05/05/flutter-principle/1_M0ik7rqmkK1Cf0xB4iwbxg.png)

widget 它是虚的，肉眼看不见，而用户真正看到 UI 的是 Element，也就是架构图蓝色的部分。可以理解为 widget 只是抽象了 element，因为在手机屏幕看到的 Container/Text 部分是不在 widget 层，它们是我们想要的数据配置。

当我们配置 Text 组件需要显示的字符串，配置输入框组件需要显示的内容的时候.在屏幕显示这些控件的时候，flutter 会根据这些信息生成 widget 控件对应的 element，同样，element 会记录这些配置信息，element 也会放到相应的 element 树。

**Flutter 中，一个 Widget 通过多次复用可以对应多个 Element 实例，Element 才是我们真正在屏幕上显示的元素。**

熟悉 Vue 的开发，应该听过**虚拟 dom**。flutter 也能体现这个概念。

widget 是不可变的，他是虚拟的组件树，而真正渲染的是 element 这棵树，如果他对应的 Widget 发生改变，它就会被标记为 dirty Element，于是下一次更新视图时根据这个状态只更新被修改的内容，从而达到提升性能的效果。

举个生动的例子就是：

> Widget 是万表大老板，他把一年要发展的战略目标（也就是配置信息）在纸上写下来，开会发给各位高管 Element，高管看到战略目标开始行动起来。但是大老板某天想改变主意，这时候它找了另外一张纸重新写。高管们为了减少工作量，需要将新的计划与旧的计划比较来作出相应的更新措施。那最后，高管们安排下面的员工 RenderObject 帮他工作。

每次，当控件挂载到控件树上时，Flutter 调用其 createElement() 方法，创建其对应的 Element。Flutter 再将这个 Element 放到元素树上，并持有创建它控件的引用，如下图：
![widget&element树](https://meandni.com/2019/05/05/flutter-principle/1_0IZb-Rcyw4yBo3CXQM5zTg.png)

## renderObject 树

RenderObject 在 Flutter 当中做组件布局渲染的工作，其为了组件间的渲染搭配及布局约束也有对应的 RenderObject 树，我们也称之为渲染树。

而 Flutter 渲染组件的顺序是:
![渲染顺序](https://www.stephenw.cc/images/render-pipeline.png)

即用户在作出**操作**后，Flutter 会有一个**动画**响应过程，伴随着 **Widget 的构建**生成基本的视图描述数据，之后的渲染阶段会根据之前的描述数据生成具体的渲染对象，然后**绘制图层**，由于直接交付给 GPU 多图层视图数据是低效率的，所以还需要进行一步**图层的合成**，最后交由引擎负责**光栅化视图**。
