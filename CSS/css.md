### 1. link 和@import 的区别

两者都是外部引用 CSS 的方式，它们的区别如下：

- link 是 XHTML 标签，除了加载 CSS 外，还可以定义 RSS 等其他事务；@import 属于 CSS 范畴，只能加载 CSS。
- link 引用 CSS 时，在页面载入时同时加载；@import 需要页面网页完全载入以后加载。
- link 是 XHTML 标签，无兼容问题；@import 是在 CSS2.1 提出的，低版本的浏览器不支持。
- link 支持使用 Javascript 控制 DOM 去改变样式；而@import 不支持。

### 2. **伪元素和伪类的区别和作用？**

- 伪元素：在内容元素的前后插入额外的元素或样式，但是这些元素实际上并不在文档中生成。它们只在外部显示可见，但不会在文档的源代码中找到它们，因此，称为“伪”元素。例如：

```css
p::before {content:"第一章：";}
p::after {content:"Hot!";}
p::first-line {background:red;}
p::first-letter {font-size:30px;}
```

- 伪类：将特殊的效果添加到特定选择器上。它是已有元素上添加类别的，不会产生新的元素。例如：

```css
a:hover {color: #FF00FF}
p:first-child {color: red}
```

**总结：**伪类是通过在元素选择器上加⼊伪类改变元素状态，⽽伪元素通过对元素的操作进⾏对元素的改变。

### 3. 对 requestAnimationframe 的理解

实现动画效果的方法比较多，Javascript 中可以通过定时器 setTimeout 来实现，CSS3 中可以使用 transition 和 animation 来实现，HTML5 中的 canvas 也可以实现。除此之外，HTML5 提供一个专门用于请求动画的 API，那就是 requestAnimationFrame，顾名思义就是**请求动画帧**。

MDN 对该方法的描述：

> `window.requestAnimationFrame()` 告诉浏览器——你希望执行一个动画，并且要求浏览器在下次重绘之前调用指定的回调函数更新动画。该方法需要传入一个回调函数作为参数，该回调函数会在浏览器下一次重绘之前执行。

**语法：** `window.requestAnimationFrame(callback);` 其中，callback 是**下一次重绘之前更新动画帧所调用的函数**(即上面所说的回调函数)。该回调函数会被传入 DOMHighResTimeStamp 参数，它表示 requestAnimationFrame() 开始去执行回调函数的时刻。该方法属于**宏任务**，所以会在执行完微任务之后再去执行。

**取消动画：**使用 cancelAnimationFrame()来取消执行动画，该方法接收一个参数——requestAnimationFrame 默认返回的 id，只需要传入这个 id 就可以取消动画了。

**优势：**

- **CPU 节能**：使用 SetTinterval 实现的动画，当页面被隐藏或最小化时，SetTinterval 仍然在后台执行动画任务，由于此时页面处于不可见或不可用状态，刷新动画是没有意义的，完全是浪费 CPU 资源。而 RequestAnimationFrame 则完全不同，当页面处理未激活的状态下，该页面的屏幕刷新任务也会被系统暂停，因此跟着系统走的 RequestAnimationFrame 也会停止渲染，当页面被激活时，动画就从上次停留的地方继续执行，有效节省了 CPU 开销。
- **函数节流**：在高频率事件( resize, scroll 等)中，为了防止在一个刷新间隔内发生多次函数执行，RequestAnimationFrame 可保证每个刷新间隔内，函数只被执行一次，这样既能保证流畅性，也能更好的节省函数执行的开销，一个刷新间隔内函数执行多次时没有意义的，因为多数显示器每 16.7ms 刷新一次，多次绘制并不会在屏幕上体现出来。
- **减少 DOM 操作**：requestAnimationFrame 会把每一帧中的所有 DOM 操作集中起来，在一次重绘或回流中就完成，并且重绘或回流的时间间隔紧紧跟随浏览器的刷新频率，一般来说，这个频率为每秒 60 帧。

**setTimeout 执行动画的缺点**：它通过设定间隔时间来不断改变图像位置，达到动画效果。但是容易出现卡顿、抖动的现象；原因是：

- settimeout 任务被放入异步队列，只有当主线程任务执行完后才会执行队列中的任务，因此实际执行时间总是比设定时间要晚；
- settimeout 的固定时间间隔不一定与屏幕刷新间隔时间相同，会引起丢帧。

### 4. Tailwind的优势
- ①默认情况下它提供了一些响应式的断点值：

```
// tailwind.config.js
module.exports = {
  theme: {
    screens: {
      'sm': '640px',
      // => @media (min-width: 640px) { ... }

      'md': '768px',
      // => @media (min-width: 768px) { ... }

      'lg': '1024px',
      // => @media (min-width: 1024px) { ... }

      'xl': '1280px',
      // => @media (min-width: 1280px) { ... }
    }
  }
}
```
你可以随时在配置文件中更改这些断点，比如你所需要的小屏幕 sm 可能指的是更小的 320px，那么你想要在小屏幕时候采用 flex 布局，还是照常写 sm:flex，遵循同样的约定，只是这个 sm 已经被你修改成适合于项目需求的值了。

- ②Tailwind 里的 spacing 掌管了 margin、padding、width 等各个属性里的代表空间占用的值，默认是采用了 rem 单位，当你在配置里这样覆写后：
```
// tailwind.config.js
module.exports = {
  theme: {
    spacing: {
      '1': '8px',
      '2': '12px',
      '3': '16px',
      '4': '24px',
      '5': '32px',
      '6': '48px',
    }
  }
}
```
你再去写 h-6（height）, m-2（margin）, mb-4（margin-bottom），后面数字的意义就被你改变了。

### 5. Tailwind CSS 缺点

- 每个 class 中要声明多个 css  class 的写法，会让人感觉代码丑陋，尤其是对于刚接触 Tailwind 的用户。然而，一段时间过后，对于绝大部分开发者来说，都是可以习惯这种写法的。
- 构建速度问题。尤其是开发阶段，当修改了 tailwind.config.js 时，如扩展新的 class，会触发Tailwind 重新构建，这个构建速度比较慢（十几秒甚至几十秒），对开发体验有一定影响。不过 Tailwind CSS v2.1 引入了 Just-in-Time Mode 特性，可以有效解决这个问题。

### 6. Tailwind CSS 的核心理念

#### 功能类（utility-first）
功能类优先是 tailwind 最重要的设计理念，上面的例子，我们做的时候我们需要一遍一遍的定义自己的 css classes 名称，甚至起名也会是个艰苦的事情，如果文件过大还需要移动到对应的文件夹，单独形成变量。
以前其实有项目我会定义比如 align-content: center ，为 .center，但别的项目可能定义为 .text-center
这个就是功能类最大的好处：

- 你不是在浪费精力去 classes 名称.
- 你的 CSS 文件停止增长
- 全局修改变得更加容易

对应 tailwind 的配置是 @tailwind utilities;
```
顺便提下这里 @tailwind base 其实就是 normalize.css，以及一些其他的默认配置，而 @tailwind component 是 tailwind 组件，我理解应该是空的（未考证）
```

####响应式设计
利用断点语法实现 @media 功能，这个操作简直逆天，以前写法都要写很多的 @media 样式，现在完全是加个前缀就可以搞定了
```
<!-- 默认长度是16, medium screens 是32,large screens 是48 -->
<img class="w-16 md:w-32 lg:w-48" src="..." />
```

w-16 对应的是 width: 4rem; 32 对应的是 8rem...

而响应式前缀可以默认可以和大部分功能类一起使用
####伪类
支持伪类的前缀标签，以及可以和响应式一起使用
```
<button
  class="bg-orange-500 hover:bg-orange-600 sm:bg-green-500 sm:hover:bg-green-600"
>
  Hover me
</button>
```
####自定义样式
由于 tailwind 的是低层次框架，现有的 tailwindcss 默认的 utility，base，component 不足以满足所有的场景；
而使用 @layer 指令，Tailwind 将自动将这些样式移动到 @tailwind base， @tailwind utility，@tailwind component 的位置
比如我们要做一个按钮组件，提炼对应的样式：
```
@layer components {
  .btn-blue {
    @apply bg-blue-500 text-white font-bold py-2 px-4 rounded;
  }
  .btn-blue:hover {
    @apply bg-blue-700;
  }
}
```