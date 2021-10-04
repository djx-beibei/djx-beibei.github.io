---
title: logical circuit
categories: [longxin_nscscc]
comments: true
---
>>龙芯杯开源代码：<http://cpu.csc-he.com/a/shiyanshebei/2019/0902/59.html>

[“龙芯杯”全国大学生计算机系统能力培养大赛——信息汇总](https://github.com/loongson-education/nscscc-wiki)

目录：

[toc]

# Verilog语法基础

>什么是Verilog

Verilog 是一种强大的硬件描述语言，以文本形式来描述数字系统硬件的结构和行为的语言。采用Verilog语言，可以在高度抽象层次上进行芯片设计。

verilog是目前应用最为广泛的硬件描述语言，可以用来进行各种层次的逻辑设计，也可以进行数字系统的逻辑综合，仿真验证和时序分析等。

可采用三种不同方式或混合方式对电路设计建模。这些方式包括：行为描述方式——使用过程化结构建模；数据流方式——使用连续赋值语句方式建模；结构化方式——使用门和模块实例语句描述建模。條萊垍頭

Verilog 语法可分为可综合语句和不可综合语句

+ 可综合语句：可转换为实际电路 例如： assign ,always,case,if-else。
+ 不可综合语句：不可以转换为实际电路  例如：wait ,time,initial。

## Verilog的抽象级别

所谓不同的抽象类别，实际上是指同一个物理电路，可以在不同层次上用Verilog语言来描述。Verilog模型可以是实际电路不同级别的抽象。这些抽象的级别和它们对应的模型类型共有以下五种：

1.系统级(system)

2.算法级(algorithmic)

3.RTL级(RegisterTransferLevel)

4.门级(gate-level)

5.开关级(switch-level)

其中，系统级和算法级是属于`行为级描述方式`的，RTL级又称为`数据流描述方式`，门级和开关级是属于`结构化描述方式`的。

1.结构化描述方式：是使用实例化低层次模块的方法，即调用其他已经定义过的低层次模块对整个电路的功能进行描述，或者直接调用Verilog内部预先定义的基本门级元件描述电路的结构。

2.数据流描述方式：是使用连续赋值语句对电路的逻辑功能进行描述，该方式特别便于对组合逻辑电路建模。

3.行为级描述方式：是使用过程块语句结构和比较抽象的高级程序语句对电路的逻辑功能进行描述。

>开关级：开关级建模是比门级建模更为低级抽象层次上的设计。在极少数情况下，设计者可能会选择使用<font color=FF0000>晶体管</font>作为设计的底层模块。随着电路设计复杂度及相关先进工具的出现，以开关为基础的数字设计慢慢步入黄昏。<font color=FF0000>MOS 管</font>用来为开关逻辑建模，数据从输入流入输出，可通过适当设置来开、关数据流。
参考：[菜鸟教程——Verilog 开关级建模](https://www.runoob.com/w3cnote/verilog2-level-modeling.html)

>门级：使用<font color=FF0000>逻辑门</font>这一级别来描述。RTL 中的寄存器和组合逻辑，其物理实现还是对应到具体门电路。但目前寄存器，组合逻辑等的电路结构基本稳定。一般EDA工具可以把RTL描述自动编译为门级描述。所以<u>一般不直接使用门级编程</u>。

在RTL级，IC可看成一组寄存器以及寄存器之间的逻辑操作构成，它可以用硬件描述语言（HDL）来描述。

RTL经过逻辑综合后，可得到门级。 

>RTL级：使用寄存器这一级别的描述方式来描述电路的数据流方式。RTL在很大程度上是<font color=FF0000>对流水线原理图的描述</font>。接近实际电路结构的描述，可以<font color=FF0000>精确描述电路的原理、执行顺序</font>等。其目的在于<font color=FF0000>可综合</font>。

>行为级：行为级是RTL级的上一层。最符合人类思维的描述方式。主要用于快速验证算法的正确性，<u>不关注电路的具体结构，不一定可以综合成实际电路结构</u>。注重算法。以直接赋值的形式进行，只关注结果。常采用大量运算，延迟等无法综合的语句。其<u>目的不在于综合，而在于算法</u>。从行为级到RTL级的转换，一般都是由IC设计人员手工翻译。

RTL级和行为级最大的区别是可综合性。

RTL级描述的目标就是可综合，而行为级描述的目标就是实现特定的功能而没有可综合的限制。

行为级的描述更多的是采取`直接赋值`的形式，只能看出结果，看不出数据流的实际处理过程。其中又大量采用算术运算，延迟等一些无法综合的语句。常常只用于`验证仿真`。RTL级的描述就会更详细一些，并且从寄存器的角度，把数据的处理过程表达出来。可以容易地`被综合工具综合成电路`的形式。

一般的综合软件都支持RTL级，行为级目前支持的不好，实用中还很少使用。所以如果是做芯片开发，都是用RTL级语言描述的，这样就不能使用比如initial块，不可使用wait语句等。这些语句一般而言是不可综合语句，但是<font color=FF00FF>在写testbench时，可大量使用行为级描述语言，这样会很方便</font>。

<font color=FF0000>综合与RTL或者行为级没有必然联系，虽然大多数行为模型不能综合</font>。比如，always 属于行为级模型，是最基本的行为模型，是可以综合的。

总结一下：
+ 算法级：主要用于快速验证算法的正确性，不一定可以综合成实际电路结构
+ 结构级：更接近电路的实际结构，电路的层次化描述，类似于电路框图
+ RTL级：贴近实际电路结构的描述，描述的细节到寄存器内容传输级别，可以精确描述电路的工作原理、执行顺序，细化到寄存器级别的结构描述也就是RTL级描述，并无绝对划分标准
+ 开关级：完整描述了电路的细节，最底层的电路描述，可以描述pmos/nmos

参考：
[行为级、RTL级、门级](https://blog.csdn.net/u012694677/article/details/89601675)
[五个级别：系统级、算法级、RTL级、门级、开关级](https://blog.csdn.net/li_hu/article/details/16962153?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-2.no_search_link&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-2.no_search_link)
[Idea->行为级描述->rtl描述->门级网标->物理版图](https://blog.csdn.net/linuxheik/article/details/88366725?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-15.no_search_link&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-15.no_search_link)

***

在RTL(register transfer language)出现之前，芯片设计使用`原理图设计`，不得不在原理图编辑器中用`实际的逻辑门`将整个数字系统搭建出来。

即我们所谓的画电路图，这里推荐几组画电路图的软件：

    multisim
        我们学校计科人当年做数电课设和个别数电实验时使用的软件，课设答辩甚至需要给老师展示电子版电路图
    
    SchemeIt
        digikey的在线工具。提供很多模板；操作也方便，直接拖动。缺点是图片导出之后，每个元件端子的细节处理得有瑕疵，一般交流问题不大，但不适合投稿

    Circuitikz
        有梯子的话，用Overleaf在线编辑即可。配合Tikz其他功能，扩展性强。
    
    visio
        Visio是教科书和芯片器件手册中的常用作图软件。灵活的标示功能，也有各种基本元器件，能够与office完美兼容。缺点是常见的芯片比较少，需要自己画。毕竟visio不是专门的电路作图软件，它在其他领域也广泛应用。
        关注微信公众号【电子硬件笔记】，里面有一些用visio画的电路图或者示意图

    立创EDA
        国产软件，在线就能用，不需要安装包，在浏览器上找到官网就能用，元件库也有很多

    亿图图示
        在线工具，适合超新手的入门级电路图设计软件，类似Visio，软件本身更符合国人的使用习惯，操作更加简单，包含丰富的图表符号，中文界面，以及各类图表模板

参考：[各位大佬画电路图用什么软件方便？](https://www.zhihu.com/question/306599515/answer/578779363)

## 寄存器传输语言（RTL）

RTL电路是最早研制成功的一种有实用价值的集成电路。而我们现在使用的也多以RTL为主，行为级拿来例化和写激励文件。

>使用RTL进行数字系统设计的优点：

1、`可以在高度抽象的层次进行数字系统设计`，如可以用 if-else 语句进行逻辑功能描述。综合工具可以根据设计者的需求，将RTL描述的电路优化成为速度最快的，逻辑资源消耗最少的逻辑电路。
 
2、`采用文本编辑工具`，而不用原理图编辑器。

>RTL级三点注意

1、RTL（Register Transfer Level）直译为寄存器传输级，顾名思义，也就是在这个级别下，描述各级寄存器（受clk约束的触发器），以及寄存器之间组合逻辑。

2、通俗来讲，RTL代码不是在“写代码”，是在画电路结构。

3、RTL代码需要“画”出输入输出端口，各级寄存器，寄存器之间的组合逻辑和前三者之间的连接。

通过RTL对同一种电路逻辑上不同程度的描述和重构，减少映射到硬件上的使用面积。这属于对电路的逻辑优化方面。如图：

![2021-08-01-nscscc1.png](https://gitee.com/jie_xue_du/learn_images_ben/raw/52444166ec935282a7c0d39558dc031c26f84b76/images/beibei_knowledge/2021-08-01-nscscc1.png)

![2021-08-01-nscscc2.png](https://gitee.com/jie_xue_du/learn_images_ben/raw/52444166ec935282a7c0d39558dc031c26f84b76/images/beibei_knowledge/2021-08-01-nscscc2.png)

# Verilog基础知识

参考资料：

[Verilog入门学习笔记](https://blog.csdn.net/weixin_39947501/article/details/112140862)

1、逻辑

四值电平逻辑：0(假)、1(真)、x(不确定)、z(高阻/浮动)

>verilog电路有4种逻辑状态0、1、Z、X对应低电平、高电平、高阻态、不确定状态
>
> x是不定，就是不确定。一般在simulation的时候出现了x，这时候就应该去注意下，当然在reset之前的ff一般都是x，实际电路里是没有x的
> 
>参考：[verilog里面有4种逻辑状态0、1、z、x](https://so.toutiao.com/s/search_wenda_pc/list?enter_from=search_result&qid=6750580513947255052&enter_answer_id=6776518563915104519&query=%E7%94%B5%E8%B7%AF0%201%20%20x%20z&qname=verilog%E9%87%8C%E9%9D%A2%E6%9C%894%E7%A7%8D%E9%80%BB%E8%BE%91%E7%8A%B6%E6%80%810%E3%80%811%E3%80%81z%E3%80%81x%EF%BC%8C%E5%88%86%E5%88%AB%E5%AF%B9%E5%BA%94%E4%BD%8E%E9%AB%98%E7%94%B5%E5%B9%B3%E9%AB%98%E9%98%BB%E6%80%81%E3%80%81%E4%B8%8D%E7%A1%AE%E5%AE%9A%E7%8A%B6%E6%80%81%EF%BC%9B%E8%AF%B7%E9%97%AE%E4%BB%80%E4%B9%88%E6%83%85%E5%86%B5%E4%B8%8B%E4%BC%9A%E5%87%BA%E7%8E%B0x%E7%8A%B6%E6%80%81%EF%BC%9F)

>Verilog的延迟：
上升延迟：在门的输入发生变化的情况下，门的输出从0，x，z变化到1所需的时间称为上升延迟；
下降延迟：下降延迟是指门的输出从1，x，z变化到0所需的时间；
关断延迟：门的输出从0，1，x变化为高阻Z所需的时间。

x和z代表的位数：十六进制-4位、八进制-3位、二进制-1位

数的最高位是x、z、0时，自动赋值扩展剩余更高位；数的最高位是1时，用0扩展剩余更高位。

2、常量

使用带基数的形式表示常量：

+ 指明位数的数字( ')：

        用十进制数表示位宽；

        基数格式包括十进制(d)、十六进制(h)、二进制(b)、八进制(o)；

        0~1、a/A ~ f/F 。

+ 不指明位数的数字：不指定基数格式时默认为十进制数；默认位宽与计算机有关(最小32位)。

3、定义标识符来表示常量

公式：parameter 参数名=常量表达式

例子：
```verilog
parameter BIT=1,BYTE=8,PI=3.14;
parameter DELAY=(BYTE+BIT)/2;
```
4、强度值：supply(驱动)、strong(驱动)、pull(驱动)、large(存储)、weak(驱动)、medium(存储)、small(存储)、highz(高阻) 按程度由强到弱排序

+ 各类线网中，只有trireg类型可以存储强度(large、medium、small三等级)

+ 强度值解决不同强度驱动源之间的赋值冲突。

+ 不同强度信号驱动同一个线网，结果服从高强度信号。同强度多个信号竞争，结果为不确定值。

5、线网类型 net type

线网net：硬件单元间的连接。由连接器件输出端连续驱动。

数据类型:wire，wand，wor，tri，triand，trior，trireg....

线网用wire声明，值由驱动源决定。默认值为z(trireg类型线网默认值为x)，默认位宽为1。

定义格式：wire[n-1:0]变量名1,变量名2,...,变量名n；
```verilog
wire a,b;   //声明两个线网类型变量a,b
wire [7:0] databus; //声明一个8位宽的线网类型变量
databuswire [32:1] busA,busB,busC; //声明3个位宽为32的线网类型变量busA,busB,busC
```
6、寄存器类型 register type

寄存器：存储元件，被改写前保持原数值。

寄存器型变量只能在initial或always内部被赋值。

寄存器未被赋值前，默认为x。

|寄存器类型|功能说明|
|:------:|:------:|
|reg|用于行为描述中对寄存器型变量的说明。无符号数|
|integer|32位带符号的整数型变量。 默认位宽是宿主机的位数，域具体实现有关，最小32位。 计算中认为是有符号的数，用二进制补码的形式储存。 不可使用位矢量integer [3:0] num;×|
|real|64位带符号的实数型变量，实数不带范围，默认为0。 用十进制或科学计数法(3e6=3000000)  当实数值被赋给一个integer型变量时，只保留整数部分的值，小数点后面的值被截掉。|
|time|64位无符号的时间型变量  时间寄存器用time来声明，宽度与具体实现有关，最小为64位。 用于存储仿真的时间，只存无符号数。每个time型变量存储一个至少64位的时间值。  调用＄time可得当前的仿真时间|

如果没有明确地说明寄存器型变量reg是多位宽的矢量，则寄存器变量的位宽为1位。

定义格式：reg[n-1:0]变量名1,变量名2,...,变量名n;
```verilog
reg clock;  //声明一个寄存器变量
clockreg [3:0] counter; //声明一个4位宽的寄存器变量counter
```

integer型变量例子：
```verilog
integer counter;    //声明一个整型变量
//counterinitial 
counter=-1; //将-1以补码的形式存储在counter中    //只有寄存器类型的变量才能在initial内部被赋值
```
real型变量例子：
```verilog
real delta; //声明一个实数型变量
//deltainitial    
begin        
    delta=4e10;  //给delta赋值        
    delta=2.13;    
end
integer i;  //声明一个整型变量
//iinitial    
i=delta;   //i得到的值是2(只将实数2.13的整数部分赋给i)
```
 time型变量例子：
```verilog
time current_time;  //声明一个事件类型的变量 
//current_timeinitial          
current_time=$time; //保存当前的仿真时间到变量current_time中
```

***

## 可综合的 Verilog —— 两种变量

>wire型

<font color=FF0000>wire型数据常用来表示用于以assign关键字指定的组合逻辑信号。</font>Verilog程序模块中输入输出信号默认为wire型。wire型信号可以用作任何方程式的输入，也可以用作“assign”语句或实例元件的输出。

wire型信号的格式同reg型信号的很类似。其格式如下：

```verilog
wire [n:0] 数据名1,数据名2,…数据名i; 
```

[n:1]代表该数据的位宽，即该数据有几位。最后跟着的是数据的名字。如果一次定义多个数据，数据名之间用逗号隔开。声明语句的最后要用分号表示语句结束。看下面的几个例子。

```verilog
wire a; //定义了一个一位的wire型数据 
wire [7:0] b; //定义了一个八位的wire型数据 
wire [4:1] c, d; //定义了二个四位的wire型数据
```
>reg型

<font color=FF0000>寄存器是数据储存单元的抽象。寄存器数据类型的关键字是reg。</font>通过赋值语句可以改变寄存器储存的值，其作用与改变触发器储存的值相当。

Verilog HDL语言提供了功能强大的结构语句使设计者能有效地控制是否执行这些赋值语句。这些控制结构用来描述硬件触发条件，例如时钟的上升沿和多路器的选通信号。reg类型数据的缺省初始值为不定值 x。

reg型数据常用来表示用于“always”模块内的指定信号，常代表**触发器**。通常，在设计中要由“always”块通过使用行为描述语句来表达逻辑关系。<font color=FF000>在“always”块内被赋值的每一个信号都必须定义成reg型。</font>

reg型数据的格式如下：

```verilog
reg [n:1] 数据名1,数据名2,… 数据名i; 
```

reg是reg型数据的确认标识符， [n:1]代表该数据的位宽，即该数据有几位（bit)。最后跟着的是数据的名字。如果一次定义多个数据，数据名之间用逗号隔开。声明语句的最后要用分号表示语句结束。看下面的几个例子：

```verilog
reg rega; //定义了一个一位的名为rega的reg型数据 
reg [3:0] regb; //定义了一个四位的名为regb的reg型数据 
reg [4:1] regc, regd; //定义了两个四位的名为regc和regd的reg型数据
```
## 可综合Verilog —— 组合逻辑

组合逻辑：输出只是当前输入逻辑电平的函数（有延时），与电路的原始状态无关。当前电路输入信号任何一个发生改变，输出都将发生改变。


在Verilog语言中，可以用两种方式描述组合逻辑 —— 使用always组合逻辑块和assign语句。

当使用always组合逻辑块描述时，变量被声明为reg型

```verilog
reg [1:0] b;
always @(*)
    begin
        if(!rst) b = 2’b0; 
        else     b = 2’b1;
    end
```

当使用assign语句描述时，变量被声明为wire

```verilog
wire [1:0] b;
assign b = (!rst) ? 2’b0 :2’b1;
```

其中，rst信号为复位信号，有效时表示赋值高电平或低电平时产生复位动作。

clk信号为时钟信号。在有效信号沿发生时刻，使写入单元的数据也有效，主要用于时序逻辑电路。

>>**时钟信号**
1.时钟信号是指有固定周期并与运行无关的信号量。
2.时钟信号是时序逻辑的基础，它用于决定逻辑单元中的状态何时更新。
3.时钟边沿触发信号意味着所有的状态变化都发生在时钟边沿到来时刻。
4.在边沿触发机制中，只有上升沿或下降沿才是有效信号，才能控制逻辑单元状态量的改变。至于到底是上升沿还是下降沿作为有效触发信号，则取决于逻辑设计。
参考：[百度百科——时钟信号](https://baike.baidu.com/item/%E6%97%B6%E9%92%9F%E4%BF%A1%E5%8F%B7/3414770?fr=aladdin#2)

## 可综合Verilog —— 时序逻辑

【~~咕咕咕~~】

时序逻辑：输出不仅是当前输入电平的函数，还与目前电路的状态有关。

由多个触发器和多个组合逻辑块组成的网络。常用的有：计数器、复杂的数据流动控制逻辑、运算控制逻辑、指令分析和操作控制逻辑。

在不同时钟信号控制的状态下，即使所有的输入都相同，其输出也不一定相同。

>同步时序和异步时序

同步时序：表示状态的寄存器组只能在唯一确定的触发条件发生时刻改变。

异步时序：表示触发条件由多个控制因素组成，如一个触发器的输出连接到另一个触发器的时钟就是异步时序逻辑。

在verilog HDL设计可综合模块时，<font color=FF0000>要避免异步时序</font>，一方面许多综合器不支持异步时序，另外异步时序很难控制由组合逻辑产生的竞争冒险。

```verilog
always @(posedge clk) begin
    if(rst==1'b0) begin
        c<=0;
    end else begin
        c<=a+b;
    end
end
```
可以看到此代码有“posedge“时钟上升沿，即表示有一个D触发器，a+b的结果c是在D触发器发出指令后才进行输出的。

组合逻辑则如下所示，是不带上升沿的，有“*”号的，直接输出a+b=c的值，不进行额外操作：

```verilog
always @(*) begin
    c=a+b;
end
```
对比两种逻辑的代码表示，可以看出同样是输出c的值，不同的逻辑输出时间却不同，时序逻辑是在时钟上升沿输出，组合逻辑则直接输出。

参考：

[组合逻辑与时序逻辑](https://blog.csdn.net/v13910/article/details/81945875)

[时序逻辑电路](https://zhuanlan.zhihu.com/p/257086967)

[组合逻辑电路和时序逻辑电路的区别](http://m.elecfans.com/article/875611.html)

[时序逻辑和组合逻辑的区别和使用](https://zhuanlan.zhihu.com/p/110543798)

***

+ 组合逻辑电路在逻辑功能上的特点是任意时刻的输出仅仅取决于该时刻的输入，与电路原来的状态无关。从这个定义中可以体会到组合逻辑电路的特点是功能上无记忆，即电路任意时刻的输出逻辑状态只取决于当前各输入逻辑状态的组合 和电路原来的逻辑状态无关系；结构上无反馈，即电路只有从输入到输出的通路 ，而没有从输出到输入的反馈回路。

+ 时序逻辑电路在逻辑功能上的特点是任意时刻的输出不仅取决于当时的输入信号，而且还取决于电路原来的状态，或者说，还与以前的输入有关。

|比较项目|组合逻辑电路|组合逻辑电路|
|:------:|:------|:------|
|输入输出关系|任意时刻的输出仅仅取决于该时刻的输入，与电路原来的状态无关|不仅仅取决于当前的输入信号，而且还取决于电路原来的状态，或者说，还与以前的输入有关|
|有无存储（记忆）单元|无（不能包含）|包含|
|结构特点|只包含门电路|组合逻辑电路+存储电路<br>输出状态必须反馈到组合电路的输入端，与输入信号共同决定组合逻辑的输出|
|分析方法|从电路的输入到输出逐级写出逻辑函数式，最后得到表示输出与输入关系的逻辑函数式。然后用公式化简法或者卡诺图化简法得到函数式的化简或变换，以使逻辑关系简单明了。<br>有时还可以将逻辑函数式转换为真值表的形式。|1.写出每个触发器的驱动方程<br>2.将驱动方程带入触发器的特性方程得到状态方程组<br>3.根据逻辑图写出电路的输出方程<br>状态转换过程描述：状态转换表、状态转换图、状态机流程图、时序图|
|设计方法|1、逻辑抽象<br>2、写出逻辑函数式<br>3、选定器件类型<br>4、将逻辑函数式化简或者变换成适当的形式<br>5、画出逻辑电路的连接图<br>6、工艺设计|1、逻辑抽象得到状态转换图或者状态转换表<br>2、状态化简<br>3、状态分配(状态编码)<br>4、选触发器求出状态方程、驱动方程和输出方程<br>5、根据方程式画出逻辑图<br>6、检查设计的电路能否自启动|
|常用组合逻辑电路|编码器<br>译码器<br>数据选择器<br>加法器<br>数值比较器|寄存器与移位寄存器<br>计数器|

[组合逻辑电路和时序逻辑电路的区别](http://www.elecfans.com/d/875611.html)

[组合逻辑电路和时序逻辑电路比较](https://blog.csdn.net/weixin_30292745/article/details/95501969)

## Verilog —— 操作符

>Verilog逻辑操作符

输出结果为1位的0和1，即true 和 false 。如果操作数是多位的，则将操作数看做整体，若操作数中每一位都是0值，则为逻辑0值，若操作数中有1，则为逻辑1值。

例如：
A=1'b1
B=1'b0
C=4'b1001
D=4'b1010

|逻辑操作符|功能|A与B的运算|C与D的运算|
|:------:|:------:|:------:|:------:|
|&&|逻辑与|A&&B=0|C&&D=1|
|\|\||逻辑或|A\| \|B=1|C\| \|D=1|
|!|逻辑非|!A=0 !B=1|!C=0 !D=0|

>Verilog位操作符

输出结果为输入变量的位数，即每一位进行与、或、非操作的运算结果。原来的操作数有几位，结果就有几位，若两个操作数位数不同，则位数短的操作数左端会自动补0。

例如：
A=1'b1
B=1'b0
C=4'b1001
D=4'b1010

|位操作符|功能|A与B的运算|C与D的运算|
|:------:|:------:|:------:|:------:|
|&|按位与|A&B=0|C&D=1000|
|\||按位或|A\|B=1|C\|D=1011|
|~|按位取反|~A=0 ~B=1|~C=0110 ~D=0101|

>Verilog缩位操作符

缩位运算符（又称归约运算符）

对变量逐位进行与、或、异或操作，输出结果为1位的0和1，即true 和 false

例如：
A=1'b1
B=1'b0
C=4'b1001
D=4'b1010

|递减操作符|功能|A与B的运算|C与D的运算|
|:------:|:------:|:------:|:------:|
|&|与|&A=1 &B=0|&C=0 &D=0|
|\||或|\|A=1 \|B=0|\|C=1 \|D=1|
|^|异或| |^C=0 ^D=0|

参考：

[verilog中的位运算符，缩位运算符和逻辑运算符的说明](http://blog.sina.com.cn/s/blog_9193a49f0102v0pk.html)

>移位操作符

移位操作符包括左移（<<），右移（>>），算术左移（<<<），算术右移（>>>）。

移位操作符是双目操作符，两个操作数分别表示要进行移位的向量信号（操作符左侧）与移动的位数（操作符右侧）。

算术左移和逻辑左移时，右边低位会补 0。
逻辑右移时，左边高位会补 0；而算术右移时，左边高位会补充符号位，以保证数据缩小后值的正确性。

例如：
```verilog
A = 4'b1100 ;
B = 4'b0010 ;

A = A >> 2;
//结果为 4'b0011 
A = A << 1;
//结果为 4'b1000 
A = A <<< 1;
//结果为 4'b1000 
C = B + (A>>>2);
//结果为 4'b0001 
```

>拼接操作符

拼接操作符用大括号 `{，}` 来表示，用于将多个操作数（向量）拼接成新的操作数（向量），信号间用逗号隔开。

拼接符操作数必须指定位宽，常数的话也需要指定位宽。例如：

```verilog
A = 4'b1010 ;
B = 1'b1 ; 

Y1 = {B, A[3:2], A[0], 4'h3 };
//结果为Y1='b1100_0011 
Y2 = {4{B}, 3'd4};
//结果为 Y2=7'b111_1100 
Y3 = {32{1'b0}};
//结果为 Y3=32h0，常用作寄存器初始化时匹配位宽的赋初值 
```

>条件操作符

条件表达式有 3 个操作符，结构描述如下：
    a? b: c

计算时，如果 a 为真（逻辑值为 1 ），则运算结果为 b ；如果 a 为假（逻辑值为 0 ），则计算结果为 c 。

例子：
```verilog
assign re = (addr[9:8] == 2'b0) ? 2’b0 : 2’b1 ; 
```

即三目运算符，可与if-else模块相转换，即assign语句和always模块的相互转换。属于优化电路逻辑【然鹅并没有优化】和降低代码查重的一个小技巧。

简单一个条件判断可以用`? :`这一 三目运算符，条件多的话建议最好不要用，用 `always` 写，否则综合器不好判断它的逻辑。

## Verilog —— 阻塞赋值和非阻塞赋值

>阻塞赋值（=）

阻塞赋值操作实质上是一次性连续完成的，即计算等号右边变量(或表达式)的值(RHS)并立即赋值给等号左边的变量(LHS)。其中阻塞的含义为在同一个always块中，当前赋值语句正在执行时禁止其后的所有其他赋值语句的执行。只有当前赋值语句执行完成后，其后的赋值语句才能被执行。

>非阻塞赋值（<=）

非阻塞赋值操作实质上是分两步完成的，即第一步：在敏感事件开始时刻(如clk正跳变沿开始时刻)开始计算等号右边变量(或表达式)的值RHS；第二步：在敏感事件结束时刻(如clk正跳变沿结束时刻)将等号右边的值赋给等号左边的变量LHS。其中非阻塞的含义为在执行当前的非阻塞赋值语句的同时允许其他的语句执行。

例如：
```verilog
always@(posedge clk)
begin
    lcd_data  <= A;
    lcd_data1 <= B;
    lcd_data2 <= C;
end
```
A,B,C在检测到高电平clk时，分别同时赋值给lcd_data、lcd_data1、lcd_data2,这就是所谓的非阻塞，意思就是同时执行。

二者主要区别为：等号右边的变量值是否立即得到更新；赋值语句执行期间是否允许其他语句执行。

阻塞赋值是立即更新完成的，所以使得电路中存在竞争冒险现象，最终导致输出不确定

总结一下：

在verilog中，阻塞赋值是按照时间逻辑进行的，即，逐步进行，每一个语句，等到上一个语句赋值结束后再进行赋值。

而在同一个always模块中，所有的非阻塞赋值都是同时赋值的，无所谓时间先后，即，并行结构。也正因如此，非阻塞赋值能根据输入逐步赋值。

>注意

用Verilog编写可综合模块时推荐遵循四条原则：

1.用always块描述时序逻辑时用非阻塞赋值(<=)。

2.用always块描述组合逻辑时用阻塞赋值(=)。

3.用always块描述时序和组合混合逻辑时用非阻塞赋值(<=)。

4.避免在多个always块中对同一变量赋值。

参考：

[Verilog中阻塞赋值与非阻塞赋值的区别](https://blog.csdn.net/weixin_38679924/article/details/102524574)

[Verilog阻塞赋值与非阻塞赋值](https://blog.csdn.net/u012373020/article/details/25097393?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1.no_search_link&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1.no_search_link)


>小贴士

[FPGA-----RTL模块基础概念（例解：异步复位、同步释放）](https://blog.csdn.net/weixin_40365277/article/details/80590842?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-2.no_search_link&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-2.no_search_link)

同步复位的缺点是处于没在时钟上升沿的时候不能成功复位

异步复位的缺点是假如持续时间比较短，会产生亚稳态（与D触发器的性质有关，D触发器需满足复位时间才能正常复位）

## Verilog —— 宏定义和文件包含

\`define & \`include 

>宏定义 define

define 可以用一个标识或者说名字代替一个复杂的名字或变量。

格式：\`define 宏名  变量或名字

在编译阶段，\`define 用于文本替换，类似于 C 语言中的 #define。

一旦 `define 指令被编译，其在整个编译过程中都会有效。例如，在一个文件中定义：
```verilog
`define RstEnable       1'b0            //复位信号有效
`define RstDisable      1'b1            //复位信号无效
`define ZeroWord        32'h00000000    //32bit 0 num
```

>文件包含 include

\`include可以在编译时将一个 Verilog 文件内嵌到另一个 Verilog 文件中，作用类似于 C 语言中的 #include 结构。该指令通常用于将全局或公用的头文件包含在设计文件里。
文件路径既可以使用相对路径，也可以使用绝对路径。

常用于将宏定义文件插入到其他Verilog文件中。在其他Verilog文件中，如果要使用宏定义内容，需要在文件开头插入文件包含语句：
```verilog
`include "define.v"
```

## Verilog —— \`timescale

在 Verilog 模型中，时延有具体的单位时间表述，并用 \`timescale 编译 指令将时间单位与实际时间相关联。该指令用于定义时延、仿真的单位和精度。

格式为：
\`timescale  时间单位/ 时间精度

例子：
```verilog
`timescale  1ns/ 1ps
```

## Verilog —— 模块声明&端口定义

module 模块名(端口 1，端口 2，端口 3，……)；

其中模块是以 module 开始，以 endmodule 结束。

模块名是模块唯一的标识符，一般建议模块名尽量用能够描述其功能的名字来命名，并且<font color=FF0000>模块名和文件名相同</font>。模块的端口表示的是模块的输入和输出口名，也是其与其他模块联系端口的标识

模块的端口可以是输入端口、输出端口或双向端口。其说明格式如下。
```verilog
//输入端口：     
input [信号位宽-1 ： 0] 端口名 1；
input [信号位宽-1 ： 0] 端口名 2；

//输出端口：     
output [信号位宽-1 ： 0] 端口名 1；
output [信号位宽-1 ： 0] 端口名 2；

//双向端口：     
inout [信号位宽-1 ： 0] 端口名 1；
inout [信号位宽-1 ： 0] 端口名 2；
```

## 用于验证的 Verilog 语法——不可综合Verilog

在Verilog中的可综合语法结构完成电路设计之后，需要对其功能进行功能验证。此时设计本身被称为DUT(design under test)。测试平台（testbench)是围绕在DUT外围的由Verilog编写的代码，testbench为DUT提供激励。

>激励文件


## 不可综合verilog语句

1.initial 
只能在test bench中使用，不能综合。
2.time
不支持time数据类型的综合。
3.force 和release
不支持force和release的综合。
4.fork join
不可综合，可以使用非块语句达到同样的效果。
5.primitives
支持门级原语的综合，不支持非门级原语的综合。
6.table
不支持UDP和table的综合。

## 实战

>RTL代码

```verilog
module test(//模块的声明
	//端口的定义
    //输入端口
	input	clk,
	input	rst,
        
    //输出端口
    output 			c,
    output reg		b,
    output reg		d
    
    );
    //always模块示例
    always @ (*) begin
        if(!rst)    d = 1'b0;//置位信号低位有效，rst=0时，将d置为0
        else        d = 1'b1;       
    end  
    
    //assign语句示例
    assign  c = (!rst) ? 1'b0 : 1'b1;
    
    //始终上升沿进行置位操作
    always @(posedge clk) begin
        if(!rst)    b = 1'b0;
        else        b = 1'b1;
    end 
endmodule
```

>激励文件
```verilog
`timescale 1ns / 1ps
module one_tb(
    );
    reg clk;
    reg rst;
    wire  b;
    wire  c;
    wire  d;
    initial begin 
        clk = 1'b0;
        rst = 1'b1;
        #100 rst = 1'b0;
    end
    
    always #5 clk = ~clk;
    always begin 
        #100
        if($time >= 150) begin 
            $display("test over!");
            $finish;
        end 
    end
    
    //模块例化
    test t0 ( 
    .clk(clk),
    .rst(rst),
    .b(b),
    .c(c),
    .d(d)
    );
endmodule
```
# 常见逻辑电路

>语言基础：verilog

>使用软件：vivado 2019.2

>用途：深入理解掌握数字电路和Verilog语法

>参考书籍：CPU设计实战

目录：

[基础逻辑门](##基础逻辑门)

[译码器](##译码器)

[编码器](##编码器)

[寄存器](##寄存器)

[多路选择器](##多路选择器)

[触发器](##触发器)

[状态机](##状态机)

参考书籍：CPU设计实战

## 基础逻辑门

```verilog
wire [7:0] a;
wire [7:0] b;

assign y1 = ~a;//反相器，非
assign y2 = a & b;//与
assign y3 = a | b;//或
assign y4 = a ^ b;//异或
assign y5 = ~(a & b);//与非
assign y6 = ~(a | b);//或非
assign y7 = ~(a ^ b);//同或
```

## 译码器

>>3-8译码器

```verilog
module decoder(
    input [2:0]in,
    output [7:0]out
);
//等于某个值，某个值即为1，否则为0
assign out[0]=(in==3'd0);
assign out[1]=(in==3'd1);
assign out[2]=(in==3'd2);
assign out[3]=(in==3'd3);
assign out[4]=(in==3'd4);
assign out[5]=(in==3'd5);
assign out[6]=(in==3'd6);
assign out[7]=(in==3'd7);
endmodule
```

根据以上代码，可以自己模仿写出其他译码器，较为复杂的逻辑可以用generate语句改善编码效率。

>>2-4译码器

```verilog
module decoder(
    input [1:0]in,
    output [3:0]out
);

assign out[0]=(in==2'd0);
assign out[1]=(in==2'd1);
assign out[2]=(in==2'd2);
assign out[3]=(in==2'd3);
endmodule
```

>>4-16译码器

```verilog
module decoder(
    input [3:0]in,
    output [15:0]out
);

assign out[0]=(in==4'd0);
assign out[1]=(in==4'd1);
assign out[2]=(in==4'd2);
assign out[3]=(in==4'd3);
assign out[4]=(in==4'd4);
assign out[5]=(in==4'd5);
assign out[6]=(in==4'd6);
assign out[7]=(in==4'd7);
assign out[8]=(in==4'd8);
assign out[9]=(in==4'd9);
assign out[10]=(in==4'da);
assign out[11]=(in==4'db);
assign out[12]=(in==4'dc);
assign out[13]=(in==4'dd);
assign out[14]=(in==4'de);
assign out[15]=(in==4'df);
endmodule
```

下述写法比较麻烦，可自行查阅资料。

>>6-64译码器

>>7-128译码器

## 编码器
>用途：根据指令译码后的结果生成ALU模块的操作码alu_op【后一种逻辑门电路设计思路】

>>8-3编码器

```verilog
module encoder(
    input [7:0] in,
    output [2:0] out
);
//在哪一位就选择哪个数字，本质上是if语句(三目运算符)
assign out = in[0] ? 3'd0 :
             in[1] ? 3'd1 :
             in[2] ? 3'd2 :
             in[3] ? 3'd3 :
             in[4] ? 3'd4 :
             in[5] ? 3'd5 :
             in[6] ? 3'd6 :
                     3'd7;
endmodule
```
优先级编码器，保证设计输入in永远至多只有一个1，即所谓的at-most-one-hot向量。

以下这个写法可以避免不必要的优先级关系：
```verilog
module encoder(
    input [7:0] in,
    output [2:0] out
);
//门电路逻辑运算，只存在与和或，存在的那一位扩展至111，与数字做与运算仍未数字，不存在即为000，直接清零该项
assign out = ({3{in[0]}} & 3'd0)
           | ({3{in[1]}} & 3'd1)
           | ({3{in[2]}} & 3'd2)
           | ({3{in[3]}} & 3'd3)
           | ({3{in[4]}} & 3'd4)
           | ({3{in[5]}} & 3'd5)
           | ({3{in[6]}} & 3'd6)
           | ({3{in[7]}} & 3'd7;
endmodule
```
## 寄存器

参考资料：

[寄存器、移位寄存器的电路原理以及verilog代码实现 ](https://www.cnblogs.com/Fun-with-FPGA/p/4711687.html)

[简单4个8位存储器读写verilog实现](https://blog.csdn.net/qq_39814612/article/details/105694701?utm_term=verilog%E5%AE%9E%E7%8E%B0%E5%AF%84%E5%AD%98%E5%99%A8%E8%AF%BB%E5%86%99&utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~all~sobaiduweb~default-1-105694701&spm=3001.4430)

【~~咕咕咕咕\~~~】
>>由D触发器组成的4位数码寄存器

逻辑功能分析：

1.异步端CR置0时，输出置0；

2.同步并行置数：D0 ~ D3为4个输入代码，当CP上升沿到达时，D0 ~ D3被同时并行置入。

3.在置数端为1，CP端为0时，保持不变。

>>移位寄存器

移位寄存器按照不同的分类方法可以分为不同的类型。 如果按照移位寄存器的移位方向来进行分类， 可以分为左移移位寄存器、移位寄存器和双向移位寄存器等；如果按照工作方式来分类，可以分为串入/串出移位寄存器、串入/并出移位寄存器和并入/串出移位寄存器等。

以下为**异步清零的4位并入串出移位寄存器**（输入为并行数据，输出为串行数据）

```verilog
module reg_bc(clk,clr,din,dout);
input clk,clr; // 输入时钟端，清零端（高电平有效）
input[3:0] din; // 数据输入端
output dout; // 数据输出端
reg[1:0] cnt; 
reg[3:0] q;
reg dout;
always@(posedge clk)  // 时钟上升沿触发
begin
    cnt<=cnt+1;  //cnt  自加 1
    if(clr) begin  // 判断清零信号是否有效
        q<=4'b0000; //q 置 置 0
    end
    else begin
        if(cnt>0) begin // 判断 cnt  是否大于 0
            q[3:1]<=q[2:0];  //q  中的值向左移 1  位
        end
        else if(cnt==2'b00) begin // 判断 cnt  是否为 0
            q<=din; //把 把 din  的值赋予 q
        end
        dout<=q[3];  //把 把 q  的最高位输出
    end
end
endmodule
```

## 多路选择器
>>五选一八位选择器
假定被选择的输入个数不是2的幂次方，同时当select输入值
```verilog
module mux5_8b(
    input [7:0] in0,in1,in2,in3,in4,
    input [2:0] sel,
    output [7:0] out
);

assign out = (sel==3'd0) ? in0 :
             (sel==3'd1) ? in1 :
             (sel==3'd2) ? in2 :
             (sel==3'd3) ? in3 :
             (sel==3'd4) ? in4 :
                           8'b0;
endmodule
```
以下这个写法可以避免不必要的优先级关系：
```verilog
module mux5_8b(
    input [7:0] in0,in1,in2,in3,in4,
    input [2:0] sel,
    output [7:0] out
);

assign out = ({8{sel==3'd0}} & in0)
           | ({8{sel==3'd1}} & in1)
           | ({8{sel==3'd2}} & in2)
           | ({8{sel==3'd3}} & in3)
           | ({8{sel==3'd4}} & in4);
endmodule
```
select信号是译码后的[位向量](https://zhuanlan.zhihu.com/p/143730700)(<http://c.biancheng.net/view/3556.html>)形式，写法如下：
```verilog
module mux5_8b(
    input [7:0] in0,in1,in2,in3,in4,
    input [4:0] sel,
    output [7:0] out
);

assign out = ({8{sel[0]}} & in0)
           | ({8{sel[1]}} & in1)
           | ({8{sel[2]}} & in2)
           | ({8{sel[3]}} & in3)
           | ({8{sel[4]}} & in4);
endmodule
```
## 触发器
>>普通的上升沿触发的D触发器
```verilog
module dff(
    input clk,
    input data_in,
    output reg q
);

always @(posedge clk) begin
    q<=data_in;
end
endmodule
```
>>带同步复位的D触发器
```verilog
module dff(
    input clk,
    input rst,//复位信号，1为复位，数据赋值为0
    input data_in,
    output reg q
);

always @(posedge clk) begin
    if(rst) q<=1'b0;
    else q<=data_in;
end
endmodule
```
```verilog
module dff(
    input clk,
    input rst,//复位信号，1为复位，数据赋值为0
    input data_in,
    output reg q
);

always @(posedge clk) begin
    q<= ~rst & data_in;
end
endmodule
```
>>带使能端的D触发器
```verilog
module dff(
    input clk,
    input en,//使能信号，1为能赋值，直接赋值即可
    input data_in,
    output reg q
);

always @(posedge clk) begin
    if(en) q<=data_in;
end
endmodule
```
```verilog
module dff(
    input clk,
    input en,//使能信号，1为能赋值，直接赋值即可
    input data_in,
    output reg q
);

always @(posedge clk) begin
    q <= en ? data_in : q;
end
endmodule
```
## 状态机
有限状态机（Finite-State Machine，FSM），简称状态机，是表示有限个状态以及在这些状态之间的转移和动作等行为的数学模型。

>用途：理解CPU设计流水的除法器模块div和乘法器模块mul

附上状态机的参考博客：

[verilog之状态机详细解释（一）](https://blog.csdn.net/woshiyuzhoushizhe/article/details/95866063)

[Verilog学习笔记一 状态机](https://www.cnblogs.com/xiaozhu5208/p/12358873.html)

>Moore 型状态机

Moore 型状态机的输出只与当前状态有关，与当前输入无关。

输出会在一个完整的时钟周期内保持稳定，即使此时输入信号有变化，输出也不会变化。输入对输出的影响要到下一个时钟周期才能反映出来。这也是 Moore 型状态机的一个重要特点：输入与输出是隔离开来的。

>Mealy 型状态机

Mealy 型状态机的输出，不仅与当前状态有关，还取决于当前的输入信号。

Mealy 型状态机的输出是在输入信号变化以后立刻发生变化，且输入变化可能出现在任何状态的时钟周期内。因此，同种逻辑下，Mealy 型状态机输出对输入的响应会比 Moore 型状态机早一个时钟周期。

>>一段式状态机

一段式状态机只选择一个状态标志位，这个状态标志位会在输入的决定下选择跳转到下一个状态还是维持原有状态，在每一个状态下检测状态标志位及输入来决定其状态的跳转及输出。其输出和状态的切换在一个always循环块中执行

>>二段式状态机

二段式状态机将状态分为当前状态和此状态，其系统会自动将次状态更新到当前状态，其输入更新在次状态上，其决定系统的状态切换和输出。其输出和状态的切换在两个个always循环块中执行，第一个always块决定系统状态标志的自动跳转，第二个always块决定系统根据不同状态下的输入进行状态的跳转及输出

>>三段式状态机

其输出和状态更新和状态切换在三个always块中，第一个always块决定系统状态标志的自动跳转，第二个always块决定系统根据不同状态下的输入进行状态的切换，第三个always块根据系统的当前状态决定输出的值。

    (0) 首先，根据状态机的个数确定状态机编码。利用编码给状态寄存器赋值，代码可读性更好。
    (1) 状态机第一段，时序逻辑，非阻塞赋值，传递寄存器的状态。
    (2) 状态机第二段，组合逻辑，阻塞赋值，根据当前状态和当前输入，确定下一个状态机的状态。
    (3) 状态机第三段，时序逻辑，非阻塞赋值，因为是 Mealy 型状态机，根据当前状态和当前输入，确定输出信号。


推荐Verilog语法教程：

[Verilog 教程基础篇](https://www.runoob.com/w3cnote/verilog-tutorial.html)
[Verilog 教程高级篇](https://www.runoob.com/w3cnote/verilog2-tutorial.html)

推荐Verilog练习题库：

[HDLBits](https://hdlbits.01xz.net/wiki/Main_Page) : <https://hdlbits.01xz.net/wiki/Main_Page>

# 课外小知识

[解析CPU架构体系的区别](https://blog.csdn.net/zhanghuaichao/article/details/89081226?utm_source=app&app_version=4.14.0&code=app_1562916241&uLinkId=usr1mkqgl919blen)

冯洛伊曼结构（也称普林斯顿结构）：X86架构

体系核心：数据和指令混在一起，统一编址。区分哪些是指令和哪些是数据大致上有以下方法：

1、用寄存器和指令周期来区分数据和指令。例如：CS段（codesegment代码段）和DS段（datasegment数据段），前者CPU是认为存放的都是指令，后者CPU认为存放的都是数据；

2、通过不同的时间段来区分指令和数据，在取指阶段取出的就是指令，执行阶段取出的就是数据。

其他参考：
[冯诺依曼体系结构](https://www.cnblogs.com/badboy200800/p/12263372.html)

哈佛结构：ARM架构

核心：数据和指令是区分开的。独立编址，就算地址一样，数据也是不一样的

[CPU的内部架构和工作原理](https://blog.csdn.net/stpeace/article/details/80101441?utm_source=app&app_version=4.14.0&code=app_1562916241&uLinkId=usr1mkqgl919blen)

CPU从逻辑上可以划分成3个模块，分别是控制单元、运算单元和存储单元，这三部分由CPU内部总线连接起来。

![2021-08-01-nscscc3.png](https://gitee.com/jie_xue_du/learn_images_ben/raw/52444166ec935282a7c0d39558dc031c26f84b76/images/beibei_knowledge/2021-08-01-nscscc3.png)

<font color=FF0000>控制单元</font>：控制单元是整个CPU的指挥控制中心，由<font color=0000FF>程序计数器PC（Program Counter）, 指令寄存器IR(Instruction Register)、指令译码器ID(Instruction Decoder)和操作控制器OC(Operation Controller)</font>等，对协调整个电脑有序工作极为重要。它根据用户预先编好的程序，依次从存储器中取出各条指令，放在指令寄存器IR中，通过指令译码(分析)确定应该进行什么操作，然后通过操作控制器OC，按确定的时序，向相应的部件发出微操作控制信号。操作控制器OC中主要包括节拍脉冲发生器、控制矩阵、时钟脉冲发生器、复位电路和启停电路等控制逻辑。

<font color=FF0000>运算单元</font>：是运算器的核心。可以执行算术运算(包括加减乘数等基本运算及其附加运算)和逻辑运算(包括移位、逻辑测试或两个值比较)。相对控制单元而言，运算器接受控制单元的命令而进行动作，即运算单元所进行的全部操作都是由控制单元发出的控制信号来指挥的，所以它是执行部件。

<font color=FF0000>存储单元</font>：包括<font color=0000FF>CPU片内缓存和寄存器组</font>，是CPU中暂时存放数据的地方，里面保存着那些等待处理的数据，或已经处理过的数据，CPU访问寄存器所用的时间要比访问内存的时间短。采用寄存器，可以减少CPU访问内存的次数，从而提高了CPU的工作速度。但因为受到芯片面积和集成度所限，寄存器组的容量不可能很大。<u>寄存器组可分为专用寄存器和通用寄存器。专用寄存器的作用是固定的，分别寄存相应的数据。而通用寄存器用途广泛并可由程序员规定其用途，通用寄存器的数目因微处理器而异。</u>

>运行原理：

![2021-08-01-nscscc4.png](https://gitee.com/jie_xue_du/learn_images_ben/raw/52444166ec935282a7c0d39558dc031c26f84b76/images/beibei_knowledge/2021-08-01-nscscc4.png)

控制单元在时序脉冲的作用下，将指令计数器里所指向的指令地址(这个地址是在内存里的)送到地址总线上去，然后CPU将这个地址里的指令读到指令寄存器进行译码。对于执行指令过程中所需要用到的数据，会将数据地址也送到地址总线，然后CPU把数据读到CPU的内部存储单元(就是内部寄存器)暂存起来，最后命令运算单元对数据进行处理加工。周而复始，一直这样执行下去，天荒地老，海枯石烂，直到停电。

具体工作流程有下面几步：

1.取指令：CPU的控制器从内存读取一条指令并放入指令寄存器。

其中，指令格式如图所示：

![2021-08-01-nscscc5.png](https://gitee.com/jie_xue_du/learn_images_ben/raw/52444166ec935282a7c0d39558dc031c26f84b76/images/beibei_knowledge/2021-08-01-nscscc5.png)

**操作码**就是汇编语言里的mov,add,jmp等符号码；**操作数**地址说明该指令需要的操作数所在的地方，是在内存里还是在CPU的内部寄存器里。

2.指令译码：指令寄存器中的指令经过译码，决定该指令应进行何种操作(就是指令里的操作码)、操作数在哪里(操作数的地址)。

3.执行指令：分两个阶段“取操作数”和“进行运算”。

4.修改指令计数器，决定下一条指令的地址。

其中，MIPS五级流水的流程为：

取指、译码、执行、访存、回写

这里嫖了几张国赛的设计流程图。

![2021-08-01-nscscc6.png](https://gitee.com/jie_xue_du/learn_images_ben/raw/52444166ec935282a7c0d39558dc031c26f84b76/images/beibei_knowledge/2021-08-01-nscscc6.png)

![2021-08-01-nscscc7.png](https://gitee.com/jie_xue_du/learn_images_ben/raw/52444166ec935282a7c0d39558dc031c26f84b76/images/beibei_knowledge/2021-08-01-nscscc7.png)

这里推荐一个公众号，【涛哥依旧】，有一些关于计算机硬件、大厂面试的经验和知识文章。

# 拓展

[vscode配置Verilog环境（Vivado+vscode）](https://blog.csdn.net/qq_43066051/article/details/110872386)