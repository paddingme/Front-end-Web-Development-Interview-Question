# 面试前端工程师

12月30日 2013年，作者 Alex MacCaw, 翻译：myownghost


注：之前我们介绍过：[一名靠谱的JavaScript程序员应备的素质](http://ourjs.com/detail/52b0fb82d6feceaa0400000b)，从程序员的角度提出要去学习哪些知识，下面这篇文章从面试官的角度介绍到面试时可能会问到的一些问题。

我在 Twitter 和 Stripe 的一部分工作内容是面试前端工程师。其实关于面试你可能很有自己的一套，这里我想跟你们分享一下我常用的方法。


不过我想先给你们一个忠告，招聘是一件非常艰巨的任务，在45分钟内指出一名侯选人是否合适是你需要完成的任务。不过面试的最大问题是每个人都会想着去雇佣他们自己，任何通过我面试的人想法大都跟我差不多（注：因为你总会问你自己关心和知道的问题），这其实不是一件好事。因此我之前的决定都有很大碰运气的成分。不过，这也是一个良好的开端。

最理想的情况下是侯选人有一个全面的 Github “简历”，这样我们可以同时通过他们的开源项目了解他们。我经常会浏览他们的代码然后针对一些特定的代码设计问一些问题。如果侯选人有非常好的开源项目记录，接下来的面试会直接去检验他们的团队协作精神。否则，我不得不去问他们一些代码方面的问题了。

我的面试非常有实践性，全部是写代码。没有抽象和理论上的东西（注：作者是个行业派），其他的面试官很可能会问这些问题，但是我认为他们前端编程的能力是值得商榷的。我问的问题大多看上去非常简单，但是每组问题都能让我考查侯选人某一方面 JavaScript 的知识。


## 第一部分：Object Prototypes (对象原型)

刚开始很简单。我会让侯选人去定义一个方法，传入一个string类型的参数，然后将string的每个字符间加个空格返回，例如：

    spacify('hello world') // => 'h e l l o  w o r l d'

尽管这个问题似乎非常简单，其实这是一个很好的开始，尤其是对于那些未经过电话面试的侯选人——他们很多人声称精通 JavaScript ，但通常连一个简单的方法都不会写。

下面是正确答案，有时侯选人可能会用一个循环，这也是一种可接受的答案。

    function spacify(str) {
      return str.split('').join(' ');
    }

接下来，我会问侯选人，如何把这个方法放入 String 对象上面，例如：

    'hello world'.spacify();

问这个问题可以让我考察侯选人是否对 function prototypes (方法原型)有一个基本的理解。这个问题会经常引起一些有意思的讨论：直接在对象的原型（prototypes）上添加方法是否安全，尤其是在 Object 对象上。最后的答案可能会像这样：

    String.prototype.spacify = function(){
      return this.split('').join(' ');
    };

到这儿，我通常会让侯选人解释一下函数声明和函数表达式的区别。


## 第二部分：参数 arguments

下一步我会问一些简单的问题去考察侯选人是否理解参数（arguments）对象。我会让他们定义一个未定义的log方法作为开始。

    log('hello world')

我会让侯选人去定义log，然后它可以代理console.log的方法。正确的答案是下面几行代码，其实更好的侯选人会直接使用 apply.

    function log(msg)　{
      console.log(msg);
    }

他们一旦写好了，我就会说我要改变我调用 log 的方式，传入多个参数。我会强调我传入参数的个数是不定的，可不止两个。这里我举了一个传两个参数的例子。

    log('hello', 'world');

希望你的侯选人可以直接使用 apply 。有时人他们可能会把 apply 和 call 搞混了，不过你可以提醒他们让他们微调一下。传入 console 的上下文也非常重要。

    function log(){
      console.log.apply(console, arguments);
    };

接下来我会让侯选人给每一个 log 消息添加一个"(app)"的前辍，比如：

    '(app) hello world'

现在可能有点麻烦了。好的侯选人知道 arugments 是一个伪数组，然后会将他转化成为标准数组。通常方法是使用 Array.prototype.slice ，像这样：

    function log(){
      var args = Array.prototype.slice.call(arguments);
      args.unshift('(app)');

      console.log.apply(console, args);
    };

## 第三部分：上下文

下一组问题是考察侯选人对上下文和 this 的理解。我先定义了下面一个例子。注意 count 属性不是只读取当前下下文的。

    var User = {
      count: 1,

      getCount: function() {
        return this.count;
      }
    };

我又写了下面几行，然后问侯选人log输出的会是什么。

    console.log(User.getCount());

    var func = User.getCount;
    console.log(func());

这种情况下，正确的答案是1和 undefined 。你会很吃惊，因为有很多人被这种最基础的上下文问题绊倒。func 是在 winodw 的上下文中被执行的，所以会访问不到count属性。我向侯选人解释了这点，然后问他们怎么样保证User总是能访问到func的上下文，即返回正即的值：1

正确的答案是使用 Function.prototype.bind ，例如：

    var func = User.getCount.bind(User);
    console.log(func());

接下来我通常会说这个方法对老版本的浏览器不起作用，然后让侯选人去解决这个问题。很多弱一些的侯选人在这个问题上犯难了，但是对于你来说雇佣一个理解apply和call的侯选人非常重要。

    Function.prototype.bind = Function.prototype.bind || function(context){
      var self = this;

      return function(){
        return self.apply(context, arguments);
      };
    }

## 第四部分：弹出窗口（Overlay library）

面试的最后一部分，我会让侯选人做一些实践，通过做一个‘弹出窗口’的库。我发现这个非常有用，它可以全面地展示一名前端工程师的技能：HTML,CSS和JavaScript。如果侯选人通过了前面的面试，我会马上让他们回答这个问题。

实施方案是由侯选人自己决定的，但是我也希望他们能通过以下几点来实现：

在遮罩中最好使用position中的fixed代替absolute属性，这样即使在滚动的时侯，也能始终让遮罩始盖住整个窗口。当侯选人忽略时我会提示他们这一点，并让他们解释fixed和absolute定位的区别。

    .overlay {
      position: fixed;
      left: 0;
      right: 0;
      bottom: 0;
      top: 0;
      background: rgba(0,0,0,.8);
    }

他们如何让里面的内容居中也是需要考察的一点。一些侯选人会选择CSS和绝对定位，如果内容有固定的宽、高这是可行的。否则就要使用JavaScript.

    .overlay article {
      position: absolute;
      left: 50%;
      top: 50%;
      margin: -200px 0 0 -200px;
      width: 400px;
      height: 400px;
    }

我也会让侯选人确保当遮罩被点击时要自动关闭，这会很好地考查事件冒泡机制的机会。通常侯选人会在overlay上面直接绑定一个点击关闭的方法。

    $('.overlay').click(closeOverlay);

这是个方法，不过直到你认识到点击窗口里面的东西也会关闭overlay的时侯——这明显是个BUG。解决方法是检查事件的触发对象和绑定对象是否一致，从而确定事件不是从子元素里面冒上来的，就像这样：

    $('.overlay').click(function(e){
      if (e.target == e.currentTarget)
        closeOverlay();
    });

## 其他方面

当然这些问题只能覆盖前端一点点的知识的，还有很多其他的方面你有可能会问到，像性能，HTML5 API, AMD和CommonJS模块模型，构造函数（constructors），类型和盒子模型（box model）。根据侯选人的情况，我经常会随机提些问题。


原文地址： [blog.sourcing.io](http://blog.sourcing.io/interview-questions?utm_source=ourjs.com)
