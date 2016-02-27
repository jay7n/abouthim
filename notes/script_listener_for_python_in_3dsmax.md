# 描述：

为了完成这次的工作，我考虑尝试了“分层”的实践 -- 尽量将要处理的各种问题分散在各个层次上，有利于降低整体的复杂度，读写和维护多少会容易一点。

在对我这个问题的理解中，具体工作是交个一个叫“ScriptPreProcessor”（脚本预处理器）的小型函数族来完成的。称它为“预”，这相对于“执行”阶段（compile - eval）而言。因为不管怎么处理，我还是最终将输出送给了source pipe去处理，差别只是对python而言，我会包上一个叫"python.execute"的字符串，里面是处理过的用户的输入。所以这个预处理器，一直是跑在maxscript的环境上。它仅仅是UI层面的一次包装。

所以在我设计的最顶层，有一个叫"ScriptPreProcessorManager"的类。它负责具体预处理器的创建/销毁/切换/对各种输入消息的处理。其中比较关键的一个想法是，对于什么时候该进行切换的判断。我认为即便作为管理器，它也不应该有这种权利，因为它管理的各种预处理器的实例，但并不知道这些类各自的互相钳制的关系（也不该知道）。所以，我通过注册callback的方式委托给client端处理，因为client端是对此问题最清楚的地方。

解下来的层次，就是ScriptPreProcessor这个继承结构。它分两个子类， Default和Extended，再之下又各分两个，分别是Singline和Multiline。这里我做的假设是，Default就是特指原有的Maxscript模式，而Extended却不假定这就是对Python模式的封装 - 因为可能以后我们会加模式，比如C#（我明白不应该超前设计，我只是认为这不算超前，因为的确有这样的声音，所以不排除这种扩展的可能性）。所以应该是Extended类制定了一些假定和规则，而python恰好满足这些设定。Singleline（为了应付mini）和Mulitiline(应付主listener窗口)也反映了我对这个问题的认识角度，它不应该对把自己放在哪个窗口环境下执行做假设，而是对自己的能力做假设。这些类配合管理器的调度，利用多态的能力实现了自己对外界环境的对接，避免了使用switch方式那种难以扩展的静态方案。

在对各种消息的处理中，这些预处理器需要能够获取当前sci窗口的环境，并对它对一些特定的操作。比如，获取当前光标的位置，当前的行数，输出提示符，等等。这些包在了这个设计的第三层 -- SimpleSciWrapper。这个类封装了一个scintilla的窗口，并通过SendMessage函数做各种与sci交互的操作。

最底层就是最原始的各种SCI消息了，通过SendMessage函数与sci环境交互。

# 自评：

我想自我评论下这个设计。

## 变与不变

有人说好的程序应该有足够的扩展性 —— 能够适应变化；有人说不应该提前设计也不该过度设计，而是立足于解决手头上的问题。这就像矛与盾一样，脱离了具体环境就失去了争辩的意义。

在我做这个事情的时候，我感受到最大的是，虽然程序员的本分是应该考虑到足够多的“变化”，但没有“不变”的东西，“变”的东西也无从开始。只有树立了哪些是“不变”的规则/假定/前提，才能围绕这个“不变”设计出“可变”的模块。只有勾勒出这个脊梁，才能围绕这个脊梁填充血肉/换皮。

而我根据对这个任务的理解，做出的规则/假定/前提是：

* 原始的Maxscirpt行为不应该变化
* 不围绕Python行为自身设计，而是围绕对它的约束设计，这包括：

    * 有提示符。对多行而言有次级提示符
    * 对多行而言有特殊执行按键，但不能是回车（回车用来换行）
    * 执行函数有回调
    * 对用户按键/鼠标/win32消息有监听
    * 执行命令完毕后会重置状态
    *   等等


* 当符合切换条件时，当前的预处理器根据条件挂起或者关闭，然后激活新的预处理器。
* 等等

这些东西（没有列完），组成了这个脊梁。它们是不应该发生变化的地方，而可以发生被定制/变化的地方在：

* 提示符可定制
* 执行按键可定制
* 消息处理函数可定制
* 判断切换的条件可定制
* 执行函数可定制
* Extended类可扩展
* 类似Python行为的实体可任意增删，这意味着

     * 如果未来需要加入C#，如果它的行为类似Python, 即可秒加。如果行为很不类似，可以扩展Extended类甚至扩展ScriptPreProcess基类。只要它可以被管理器纳入管理，对于切换的问题便可以无视。

* 等等

这个方向就是我认识这个问题的角度。是我抽象的根据。它可能与功能设计者的角度不一样。而这些设计/实现，我也没有办法在一开始计划的时候想清楚。而且如果任务描述给出的越少，我抽象的空间也就越大，而最终结果和功能设计者相差的也可能越大。所以任务描述还是应该做到最大限度的详细。越限制我能抽象的空间，就越能让我抽象出正确的解决问题的方案。

## 最棘手的问题

最棘手的问题来自这么几个：

### 对我们现有的代码是如何与Scintilla环境进行交互的，没有充分详细的认识。
因为进度与时间的压力，我只是草草搞清楚listener上那些字符是如何打印上去的，便开始了我的工作，结果在这种过程中遭遇了相当的麻烦。简直是，噩梦。

具体而言是对wm_keydown和wm_char消息的处理，由于对此两消息的接连处理，导致在我的处理函数中要判断当前环境时，要键入的字符还未进入sci环境，或者已然被删除了（vk_back时）；如果我等到sci处理完再处理，又已经晚了。所以我必须在这过程中想办法先“还原”应该的字符串是什么，才能进行处理。意识到这个问题的时候，框架已经搭的差不多了。所以发现这个问题时非常绝望，最后想出来了用botchCurrentLineWithPressedKey()函数弥补。这也造成了这个架构中散落一些botch的字眼，本来挺干净的 设计，就这样被抹脏。

### 将新代码整合进老代码的难度
还是因为进度与时间的压力，老旧的代码又臭又长，不可能全部读完，我只能挑一些直观影响到我工作的代码理解，并想办法把我写的模块整合进来。

但由于依然还是有庞大的陌生代码，而我影响/影响我的一些全局变量又充斥在那些陌生的地方，导致有相当的地方充满了“神秘感”，让我畏手畏脚，不敢轻易重构那些地方。结果只能是半猜半碰运气做些补丁方案。这种感觉很像在蛮荒时代探险的野人，处处没有安全感。为了调试这些整合的结果，我估计不停启动max的次数就不下几十次。举个例子，update_mini()函数是它的一个原始函数，它被用在很多地方，而它自己又会影响/改变 mini 窗口的很多结果（直接在里面塞sci的API）。对于这个问题，如果理想情况下，我会一个个分析它出现的地方，并尝试将它封装的能够兼容我的管理系统。但现实是情况不理想，所以我只能进行黑盒测试，在max里面去试，然后在出错的地方加保护（listener->set_block_mini_updates()）。到现在，我都不确定还有哪些地方会因为它而出问题。

### 很难穷举出用户可能做出的操作的排列组合
这个任务发生在前端，但特殊之处是，融合了非常多的用户操作的可能性（切换按钮和提示符的互操作，主窗口和迷你窗口的互操作）。所以大部分code都是围绕这些可能性做出的处理。

### 自己设计的失误
有那么一段时间我曾为我把“判断何时切换模式”的功能委托给外部环境处理的决定而感到牛逼，直到我发现，当需要真的切换时，不管需要“是不是”的信息，还有一些其它的信息，比如，上面第一个问题谈到的，sci尚未被处理的数据。而这些数据，貌似又是因情况而定，在这个一个由管理器接管的high-level的层次上传递具体的各种类型的数据，显然不合适。这时我才发现有很多我没考虑到的情况。而这些情况，大部分又是由第一个问题引起的。虽然最后还是搞定了，但代价是又花了很多时间成本，以及不那么漂亮的设计。

## 对进度/时间管理的想法

过去的两个sprint，算起来一个月，是我入公司以来加班强度最厉害的时期之一。每天我都工作到很晚，不止一次加班到深更半夜。这个sprint中，我代码总共写下来超过2k+ 行。很难想象这会发生在我们这种公司的文化中。如果要批评，我会主动承担一部分责任，比如能力不行，水平差，倾向于“过度设计派”，倾向于复杂地想问题但又没有简洁的解决问题的能力。但我仍然觉得，即便这样，管理中还是有让我不理解的地方。

### 我觉得最大的问题就是对这个任务复杂度的估计严重不足。
作为刚刚进入max, 没理解listener有什么行为, 没做过什么win32开发，听都没听过scintilla是什么玩意的我（们），原计划要在一个sprint（10天）中实现这个功能，现在结合上面我提到的“最棘手的问题”， 想想真的算是“天方夜谭”。即便作为一个月，我都觉得勉强。但“一个月了都搞不定，说什么都说不过去”这种想法的压力，只能让我硬着头皮硬上。做得那么多辛苦却还有受到负面评价的风险，这种心情的滋味可真不怎么样。而且有些问题带来的复杂度，本身就是因为“急”这个动机导致的。因为着急，所以只能先这么着了，虽然明明觉得这样是不对的 。因为着急，已经没有那种淡定的心情考虑某种实现的风险/收益，只能像鸵鸟一样假装那些问题不存在，直到某一天彻底崩掉。在plan meeting/scrum meeting上，对提出的问题，也会收到不理解/不信任的反馈，这又多多少少打击了想要真诚沟通问题的意愿。

### 对story的理解困惑
上周五meeting的时候我提出了这个问题。我没有接受过scrum的培训，不知道这些知识。所以也很好奇。简单说，我的困惑是，如果story同时满足这两个条件：

* Story的创建只能以用户衡量为导向
* Story要能够在短时间内可控（1-2个sprint）

那我认为它就有问题。因为这两个条件是很有可能互相矛盾的。如果只能创建用户故事，那么用户一个简单的操作，实现起来啊也很有可能要下很大功夫，一两个spirnt肯定完不成（不满足条件二）。 必定需要细分（不是切除sub task， 这种粒度不够）。而细分的话，很可能会整个整个sprint做一些用户层面看不到的代码（但这又违反了条件一）。用户看不到，也就没法测，没法测也就没法关story。所以这种story也不会被 允许创建的。那么，这种任务到底该怎么做？举我这个例子，结果就是硬生生在两个sprint里面连架构带结果统统呈现出来。

另外一个例子，去年做Stignray我遇到的，QA反馈说Color Picker 选好的颜色，在引擎中看不到。这是作为一个bug报的，我看了半天代码，发现这是前端（js）和中转端(c#) 对数据结构上的设计很不匹配造成的，而其架构也不容易“适配”。势必要做些high-level的调整才能适应，但这绝非一朝一夕能搞定。首先这个bug已经超出了“修”的时间范围，而且其次作为一个用户级别的Story, 它也不可能做完。它也没有办法再在用户层面上切分了 —— 要求就是点这个颜色，在viewport里能看到那个颜色的变化。这个任务其实需要先扭转架构，才能实现。而扭转架构又需要一些连续的时间片。我想问的问题是，如果放到我们这里的管理模式，该怎么解决 ？