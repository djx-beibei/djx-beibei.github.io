---
title: logical circuit
categories: [longxin_nscscc]
comments: true
---
>>龙芯杯开源代码：<http://cpu.csc-he.com/a/shiyanshebei/2019/0902/59.html>

[“龙芯杯”全国大学生计算机系统能力培养大赛——信息汇总](https://github.com/loongson-education/nscscc-wiki)



目录：

[toc]



***

推荐Verilog语法教程：

[Verilog 教程基础篇](https://www.runoob.com/w3cnote/verilog-tutorial.html)
 	 [Verilog 教程高级篇](https://www.runoob.com/w3cnote/verilog2-tutorial.html)

推荐Verilog练习题库：

[HDLBits](https://hdlbits.01xz.net/wiki/Main_Page) : <https://hdlbits.01xz.net/wiki/Main_Page>

Verilog 在线评测：

<https://verilogoj.ustc.edu.cn/oj/>

***



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
+ integer类型变量用于循环变量和计数

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

`默认为高阻值z`

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

`默认为不定值x`

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
Y3 = {32{1'b0} };
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

input output可以指定为reg型或者wire型

一般默认为wire类型

凡是begin。。。end 或者 fork。。。join中的变量，通通都是reg型变量，initial和always中的变量也都是reg，在可综合代码风格中，其他的基本上就是wire型了

## 用于验证的 Verilog 语法——不可综合Verilog

在Verilog中的可综合语法结构完成电路设计之后，需要对其功能进行功能验证。此时设计本身被称为DUT(design under test)。测试平台（testbench)是围绕在DUT外围的由Verilog编写的代码，testbench为DUT提供激励。

>激励文件

参考：[菜鸟教程——Verilog 仿真激励](https://www.runoob.com/w3cnote/verilog-testbench.html)

![2021-11-1-testbench](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-11-1-testbench.png)

 testbench 最基本的结构包括信号声明、激励和模块例化。

+ 根据设计的复杂度，需要引入时钟和复位部分。当然更为复杂的设计，激励部分也会更加复杂。根据自己的验证需求，选择是否需要自校验和停止仿真部分。

+ 当然，复位和时钟产生部分，也可以看做激励，所以它们都可以在一个语句块中实现。也可以拿自校验的结果，作为结束仿真的条件。



> 具体分析

1. 信号声明

testbench 模块声明时，一般不需要声明端口。因为激励信号一般都在 testbench 模块内部，没有外部信号。

声明的变量应该能全部对应被测试模块的端口。被测试模块输入端对应的变量应该声明为 `reg` 型，如 clk，rstn 等，输出端对应的变量应该声明为 `wire` 型，如 dout，dout_en。

2. 时钟生成

生成时钟的方式有很多种，例如以下两种生成方式也可以借鉴：

```verilog
//round 1
initial clk = 0 ;
always #10 clk = ~clk;

//round 2
initial begin
    clk = 0 ;
    forever begin
        #10 clk = ~clk;
    end
end       
```

利用取反方法产生时钟时，一定要给 clk 寄存器赋初值

3. 复位生成

复位逻辑比较简单，一般赋初值为 0，再经过一段小延迟后，复位为 1 即可。

这里大多数的仿真都是用的低有效复位。比赛用的为低电平同步复位，即低电平有效。但《自己动手写CPU》及课堂教学一般为高电平有效复位。

4. 激励部分

$readmemh从文件data中读取数据

5. 模块例化

利用 testbench 开始声明的信号变量，对被测试模块进行例化连接。

6. 自校验

在数据输出使能 dout_en 有效时，对输出数据 dout 与参考数据 read_temp（激励部分产生）做一个对比，并将对比结果置于信号 err_cnt 中。最后就可以通过观察 err_cnt 信号是否为 0 来直观的对设计的正确性进行判断。

比如比赛使用的debug的3~4条信号即为自校验过程。

7. 结束仿真

$finish

停止仿真之前，可以将自校验的结果，通过系统任务 $display 在终端进行显示。


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

译码是[编码](https://baike.baidu.com/item/编码/80092)的逆过程，同时去掉[比特流](https://baike.baidu.com/item/比特流/6435599)在传播过程中混入的噪声。利用译码表把文字译成一组组数码或用译码表将代表某一项信息的一系列[信号](https://baike.baidu.com/item/信号/32683)译成文字的过程称之为译码。

**译码器**是[电子技术](https://baike.baidu.com/item/电子技术/2470)中的一种多输入多输出的[组合逻辑电路](https://baike.baidu.com/item/组合逻辑电路/2083023)，负责将[二进制](https://baike.baidu.com/item/二进制)[代码](https://baike.baidu.com/item/代码)翻译为特定的对象（如[逻辑电平](https://baike.baidu.com/item/逻辑电平)等），功能与[编码器](https://baike.baidu.com/item/编码器/6029803)相反。译码器一般分为通用译码器和数字显示译码器两大类。

数字电路中，译码器（如n线－2n线[BCD](https://baike.baidu.com/item/BCD)译码器）可以担任[多输入多输出](https://baike.baidu.com/item/多输入多输出)[逻辑门](https://baike.baidu.com/item/逻辑门/5141155)的角色，能将已编码的输入转换成已编码的输出，这里输入和输出的编码是不同的。输入使能信号必须接在译码器上使其正常工作，否则输出将会是一个无效的码字。译码在[多路复用](https://baike.baidu.com/item/多路复用/1180849)、 [七段数码管](https://baike.baidu.com/item/七段数码管/927592)和[内存](https://baike.baidu.com/item/内存)地址译码等应用中是必要的。

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
## 组合逻辑电路的应用：三人举手表决电路

通过卡诺图

得逻辑表达式：
				$ result = AB + AC $
				$ result = ( a \& b ) | ( a \& c) $

RTL代码：
```verilog
`timescale 1ns / 1ps
module test(
    input wire a,
    input wire b,
    input wire c,
    output reg result
);
    
always @(a or b or c) begin
    result=(a&b)|(a&c);
end
endmodule

```

激励文件：
```verilog
`timescale 1ns / 1ps
module test_tb(

    );
    reg a;
    reg b;
    reg c;
    wire result;
    test t0(.a(a),.b(b),.c(c),.result(result));
    initial begin
        a=0;
        b=0;
        c=0;
        #10
        a=0;
        b=0;
        c=1;
        #10
        a=0;
        b=1;
        c=0;
        #10
        a=0;
        b=1;
        c=1;
        #10
        a=1;
        b=0;
        c=0;
        #10
        a=1;
        b=0;
        c=1;
        #10
        a=1;
        b=1;
        c=0;
        #10
        a=1;
        b=1;
        c=1;
        #10 $finish;
    end
    initial begin
        $monitor($time,".a=%b,b=%b,c=%b,result=%b",a,b,c,result);
    end
endmodule
```

```verilog
`timescale 1ns / 1ps
module test_tb(

    );
    reg a;
    reg b;
    reg c;
    wire result;
    test t0(.a(a),.b(b),.c(c),.result(result));
    
    always #10 {a,b,c}={a,b,c}+3'b1;
    initial begin
        a=0;
        b=0;
        c=0;
    end
    initial begin
        $monitor($time,".a=%b,b=%b,c=%b,result=%b",a,b,c,result);
    end
endmodule
```
仿真波形图：

![2021-10-30-handout](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-30-handout.png)



***



## 锁存器

参考：

[百度百科——锁存器](https://baike.baidu.com/item/%E9%94%81%E5%AD%98%E5%99%A8/10801965?fr=aladdin)

锁存器（latch）---对脉冲电平敏感，在时钟脉冲的电平作用下改变状态

锁存器是电平触发的存储单元，数据存储的动作取决于输入时钟（或者使能）信号的电平值，仅当锁存器处于`使能(en)状态`时，输出才会随着数据输入发生变化。

锁存器不同于触发器，它不在锁存数据时，输出端的信号随输入信号变化，就像信号通过一个缓冲器一样；一旦锁存信号起锁存作用，则数据被锁住，输入信号不起作用。锁存器也称为透明锁存器，指的是不锁存时输出对于输入是透明的。激励信号的任何变化，都将直接引起锁存器输出状态的改变，很有可能会因为瞬态特性不稳定而产生`振荡现象`。

***

触发器（Flip-Flop，简写为FF），也叫双稳态门，又称双稳态触发器。是一种可以在两种状态下运行的数字逻辑电路。触发器一直保持它们的状态，直到它们收到输入脉冲，又称为触发。当收到输入脉冲时，触发器输出就会根据规则改变状态，然后保持这种状态直到收到另一个触发。

触发器（flip-flop），是边沿敏感的存储单元，数据存储的动作（状态转换）由某一信号的`上升沿或者下降沿`进行同步的（限制存储单元状态转换在一个很短的时间内）。

***

寄存器（register），在 Verilog  中用来暂时存放参与运算的数据和运算结果的变量。一个变量声明为寄存器时，它既可以被综合成触发器，也可能被综合成 Latch，甚至是 wire  型变量。但是大多数情况下我们希望它被综合成触发器，但是有时候由于代码书写问题，它会被综合成不期望的 Latch 结构。

***

Latch 的主要危害有：

1）输入状态可能多次变化，容易产生毛刺，增加了下一级电路的不确定性；

2）在大部分 FPGA 的资源中，可能需要比触发器更多的资源去实现 Latch 结构；

3）锁存器的出现使得静态时序分析变得更加复杂。

Latch 多用于门控时钟（clock gating）的控制，一般设计时，我们应当避免 Latch 的产生。

参考：[Verilog 避免 Latch](https://www.runoob.com/w3cnote/verilog-latch.html)

***



+ RS锁存器

[锁存器](https://blog.csdn.net/weixin_37753976/article/details/98466268)

[SR锁存器](https://blog.csdn.net/phenixyf/article/details/8727775)

+ D锁存器

为了解决RS锁存器带来的问题（RS不能同时为1），在此基础上，添加两个与门和一个非门，即可避免这种情况

但是D锁存器同样存在它的问题，那就是无法去除输入的毛刺（换句话说，对毛刺很敏感）。可以看到当E端为0的时候，R端也会恒为0，S端则等于D端输入，亦即是此时输出直接等于输入。所以在E=0的时候，输出完全跟随输入（哪怕输入存在毛刺/抖动，这在电路中十分常见！！！）。为了进一步的改进，人们在此基础上又提出了D触发器。

[RS锁存器，D锁存器、D触发器简介](https://blog.csdn.net/kewei168/article/details/101141511)

参考：

[触发器和锁存器的区别-1](https://zhidao.baidu.com/question/935990923659887332.html)

[锁存器和触发器的区别-2](http://www.elecfans.com/dianzichangshi/20171102573904.html)

锁存器同其所有的输入信号相关，当输入信号变化时锁存器就变化，没有时钟端；触发器受时钟控制，只有在时钟触发时才采样当前的输入，产生输出。

锁存器由电平触发，非同步控制。在使能信号有效时锁存器相当于通路，在使能信号无效时锁存器保持输出状态。触发器由时钟沿触发，同步控制。

锁存器对输入电平敏感，受布线延迟影响较大，很难保证输出没有毛刺产生；触发器则不易产生毛刺。

拓展：

[毛刺的概念](https://wenku.baidu.com/view/948a19a326fff705cd170a1f.html)

***

锁存的产生：

1. if结构不完整

组合逻辑中，不完整的 if - else 结构，会产生 latch。

例如下面的模型，if 语句中缺少 else 结构，系统默认 else 的分支下寄存器 q 的值保持不变，即具有存储数据的功能，所以寄存器 q 会被综合成 latch 结构。

```verilog
module latch(
    input       data,
    input       en ,
    output reg  q
) ;  
    always @(*) begin
        if (en) q = data ;
    end
endmodule
```

避免此类 latch 的方法主要有 2 种，一种是补全 if-else 结构，或者对信号赋初值。

例如，上面模型中的always语句，可以改为以下两种形式：

```verilog
    // 补全条件分支结构    
    always @(*) begin
        if (en)  q = data ;
        else     q = 1'b0 ;
    end

    //赋初值
    always @(*) begin
        q = 1'b0 ;
        if (en) q = data ; //如果en有效，改写q的值，否则q会保持为0
    end
```



但是在时序逻辑中，不完整的 if - else 结构，不会产生 latch，例如下面模型。

这是因为，q 寄存器具有存储功能，且其值在时钟的边沿下才会改变，这正是触发器的特性。

```verilog
module ff(
    input       clk ,
    input       data,
    input       en ,
    output reg  q
) ;   
    always @(posedge clk) begin
        if (en) q <= data ;
    end
endmodule
```

2. case结构不完整

case 语句产生 Latch 的原理几乎和 if 语句一致。在组合逻辑中，当 case 选项列表不全且没有加 default 关键字，或有多个赋值语句不完整时，也会产生 Latch

```verilog
module latch(
    input       data1,
    input       data2,
    input [1:0] sel ,
    output reg  q 
) ;
    always @(*) begin
        case(sel)
            2'b00:  q = data1 ;
            2'b01:  q = data2 ;
        endcase
    end
endmodule
```

当然，消除此种 latch 的方法也是 2 种，将 case 选项列表补充完整，或对信号赋初值。

补充完整 case 选项列表时，可以罗列所有的选项结果，也可以用 default 关键字来代替其他选项结果。

例如，上述 always 语句有以下 2 种修改方式。

```verilog
    //用 default 关键字来代替其他选项结果
	always @(*) begin
        case(sel)
            2'b00:    q = data1 ;
            2'b01:    q = data2 ;
            default:  q = 1'b0 ;
        endcase
    end
	//罗列所有的选项结果
    always @(*) begin
        case(sel)
            2'b00:  q = data1 ;
            2'b01:  q = data2 ;
            2'b10, 2'b11 :  
                    q = 1'b0 ;
        endcase
    end
```

3. 原信号赋值或判断

在组合逻辑中，如果一个信号的赋值源头有其信号本身，或者判断条件中有其信号本身的逻辑，则也会产生 latch。因为此时信号也需要具有存储功能，但是没有时钟驱动。此类问题在 if 语句、case 语句、问号表达式中都可能出现，例如：

```verilog
    //signal itself as a part of condition
    reg a, b ;
    always @(*) begin
        if (a & b)  a = 1'b1 ;   //a -> latch
        else a = 1'b0 ;
    end
   
    //signal itself are the assigment source
    reg        c;
    wire [1:0] sel ;
    always @(*) begin
        case(sel)
            2'b00:    c = c ;    //c -> latch
            2'b01:    c = 1'b1 ;
            default:  c = 1'b0 ;
        endcase
    end

    //signal itself as a part of condition in "? expression"
    wire      d, sel2;
    assign    d =  (sel2 && d) ? 1'b0 : 1'b1 ;  //d -> latch
```

避免此类 Latch 的方法，即在组合逻辑中避免这种写法，信号不要给信号自己赋值，且不要用赋值信号本身参与判断条件逻辑。

例如，如果不要求立刻输出，可以将信号进行一个时钟周期的延时再进行相关逻辑的组合。上述第一个产生 Latch 的代码可以描述为：

```verilog
    reg   a, b ;
    reg   a_r ;
   
    always (@posedge clk)
        a_r  <= a ;
       
    always @(*) begin
        if (a_r & b)  a = 1'b1 ;   //there is no latch
        else a = 1'b0 ;
    end
```



4. 敏感信号列表不完整

如果组合逻辑中 always@() 块内敏感列表没有列全，该触发的时候没有触发，那么相关寄存器还是会保存之前的输出结果，因而会生成锁存器。

这种情况，把敏感信号补全或者直接用 always@(*) 即可消除 latch。

总之，为避免 latch 的产生，在组合逻辑中，需要注意以下几点：

- 1）if-else 或 case 语句，结构一定要完整
- 2）不要将赋值信号放在赋值源头，或条件判断中
- 3）敏感信号列表建议多用 always@(*)




## 触发器

参考：

[百度百科——触发器](https://baike.baidu.com/item/%E8%A7%A6%E5%8F%91%E5%99%A8/193146?fr=aladdin)

[IC基础知识（十五）RS触发器、JK触发器、D触发器、T触发器](https://blog.csdn.net/Andy_ICer/article/details/111538705?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-2.no_search_link&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-2.no_search_link)

在实际的数字系统中往往包含大量的存储单元，而且经常要求他们在同一时刻同步动作，为达到这个目的，在每个存储单元电路上引入一个时钟脉冲（CLK）作为控制信号，只有当CLK到来时电路才被“触发”而动作，并根据输入信号改变输出状态。把这种在时钟信号触发时才能动作的存储单元电路称为触发器，以区别没有时钟信号控制的锁存器。

按逻辑功能不同分为：RS触发器、D触发器、JK触发器、T触发器。

按触发方式不同分为：电平触发器、边沿触发器和脉冲触发器。

按电路结构不同分为：基本RS触发器和钟控触发器。

按存储数据原理不同分为：静态触发器和动态触发器。

按构成触发器的基本器件不同分为：双极型触发器和MOS型触发器。

***

逻辑功能，是指触发器的次态和现态及输入信号之间在稳态下的逻辑关系。这种逻辑关系可以用特性表、特性方程或状态转换图给出。

* [x] 特性表
* [x] 特性方程
* [x] 状态转换图

根据逻辑功能的不同特点，把触发器分为RS、JK、T、D等几种类型。

+ RS触发器（基本、同步、主从）
+ JK触发器
+ T触发器
+ D触发器

电路结构，是指电路中门电路的种类及组合方式。
基本RS触发器、同步RS触发器、主从触发器、边缘触发器等是指电路结构的不同形式。由于电路结构形式的不同，带来了各不相同的动作特点。

同一种逻辑功能的触发器可以用不同的电路结构实现。反过来说，用同一种电路结构形式可以作成不同逻辑功能的触发器。

参考：

[边沿触发器](https://baike.baidu.com/item/%E8%BE%B9%E6%B2%BF%E8%A7%A6%E5%8F%91%E5%99%A8/2214312?fr=aladdin)


***

RS触发器

[什么是数字电路中的RS触发器？](http://www.elecfans.com/d/1531932.html)

***

JK触发器

[触发器详解——（二）JK触发器](https://blog.csdn.net/qq_41844618/article/details/104347445)

RTL文件：
```verilog
`timescale 1ns / 1ps
module diff(
     clk,j,k,q,qb
);
input wire  clk,j,k;
output wire qb;
output reg  q;
assign qb = ~q;
always@(posedge clk)begin
    case({j,k})
    2'b00:    q <= q;
    2'b01:    q <= 0;
    2'b10:    q <= 1;
    2'b11:    q <= ~q;
    endcase
end
endmodule
```

激励文件：
```verilog
module diff_tb;
    reg clk,j,k;
    wire q,qb;
    
    always #20 {j,k} = {j,k} + 2'b1;
    initial
        begin
            clk <= 0;
            j <= 0;
            k <= 0;
        end
        
    always #10 clk <= ~clk;//半周期为20ns,全周期为40ns的一个信号

    diff d1(.clk(clk),.j(j),.k(k),.q(q),.qb(qb));
endmodule
```

仿真波形图：

![2021-10-15-jk_diff](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-15-jk_diff.png)


***

T触发器

RTL文件：
```verilog
`timescale 1ns / 1ps
module diff(
    clk,rst,t,q,qb
    );
    input wire  clk,t,rst;
    output wire qb;
    output reg  q;
    assign qb = ~q;
    always@(posedge clk)begin
        if(rst)     q <= 0;
        else if(t)  q <= ~q;
     end  
endmodule
```

激励文件：
```verilog
module diff_tb;
    reg clk,t,rst;
    wire q,qb;
    
    always #200 t = ~t;
    initial
        begin
            clk <= 0;
            rst <= 1;
            t <= 0;

            #20 rst = 0;    
            
        end
    always #10 clk <= ~clk;//半周期为20ns,全周期为40ns的一个信号
    diff d1(.t(t),.rst(rst),.clk(clk),.q(q),.qb(qb));
endmodule
```

仿真波形图：

![2021-10-15-t_diff](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-15-t_diff.png)

***

D触发器

>>普通的上升沿触发的D触发器

RTL文件：
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

激励文件：
```verilog
module dff_tb;
    reg clk,data_in;
    wire q;
    
    always #20 data_in = ~data_in;
    initial
        begin
            clk <= 0;
            data_in <= 0;            
        end
    always #10 clk <= ~clk;//半周期为20ns,全周期为40ns的一个信号
    dff d1(.clk(clk),.data_in(data_in),.q(q));
endmodule

```

仿真波形图：

![2021-10-15-d_diff1](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-15-d_diff1.png)


>>带低电位有效异步复位端的触发器

RTL文件：
```verilog
module dff(
    input clk,
    input rst,//复位信号，1为复位，数据赋值为0
    input data_in,
    output reg q
);

always @(posedge clk or negedge rst) begin
    if(rst) q<=1'b0;
    else q<=data_in;
end
endmodule
```

激励文件：
```verilog
module dff_tb;
    reg clk,rst,data_in;
    wire q;
    
    always #20 data_in = ~data_in;
    initial
        begin
            clk <= 0;
            rst <= 1;
            data_in <= 0;
        end
        
    always #10 clk <= ~clk;//半周期为20ns,全周期为40ns的一个信号
    always #40 rst <= ~rst;
    dff d1(.clk(clk),.rst(rst),.data_in(data_in),.q(q));
endmodule
```

仿真波形图：

![2021-10-15-d_diff2](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-15-d_diff2.png)


>>带同步复位的D触发器

在时钟上升沿和复位信号同时有效时，才可以复位
RTL文件：
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

激励文件：
```verilog
module dff_tb;
    reg clk,rst,data_in;
    wire q;
    
    always #20 data_in = ~data_in;
    initial
        begin
            clk <= 0;
            rst <= 1;
            data_in <= 0;
        
        end
        
    always #10 clk <= ~clk;//半周期为20ns,全周期为40ns的一个信号
    always #50 rst <= ~rst;
    dff d1(.clk(clk),.rst(rst),.data_in(data_in),.q(q));
endmodule
```

仿真波形图：

![2021-10-15-d_diff3](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-15-d_diff3.png)

>>带使能端的D触发器

RTL文件：
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


激励文件：
```verilog
module diff_tb;
    reg clk,en,data_in;
    wire q;
    
    always #20 data_in = ~data_in;
    initial
        begin
            clk <= 0;
            en <= 1;
            data_in <= 0;
        end
        
    always #10 clk <= ~clk;//半周期为20ns,全周期为40ns的一个信号
    always #40 en <= ~en;
    diff d1(.clk(clk),.en(en),.data_in(data_in),.q(q));
endmodule

```

仿真波形图：

![2021-10-15-d_diff4](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-15-d_diff4.png)

## 时序逻辑电路的应用：计数器

h 十六进制

d 十进制

o 八进制

b 二进制



RTL代码：

```verilog
`timescale 1ns / 1ps
module test(
    input wire clk,
    input wire rst,
    output reg [3:0] cout,
    output reg carry
    );
    always @(posedge clk) begin
        if(rst)begin
            cout<=4'd0;
        end else begin
            cout<=cout+4'd1;
        end
    end
    always @(posedge clk) begin
        if(cout==4'd12)begin//4'b1100
            carry<=1'b1;
            cout<=4'd0;
        end else begin
            carry<=1'b0;
        end
    end
endmodule
```



激励文件：

```verilog
`timescale 1ns / 1ps
module test_tb(

    );
    reg clk;
    reg rst;
    wire [3:0] cout;
    wire carry;

    test t0(.clk(clk),.rst(rst),.cout(cout),.carry(carry));
    initial begin
        clk<=0;
        rst<=0;
        #20 rst<=1;
        #20 rst<=0;
    end
    always #10 clk<=~clk;
endmodule
```



仿真结果如下图所示：

![2021-10-24-cal](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-24-cal.png)




## 状态机

有限状态机（Finite-State Machine，FSM），简称状态机，是表示有限个状态以及在这些状态之间的转移和动作等行为的数学模型。

>用途：理解CPU设计流水的除法器模块div和乘法器模块mul

附上状态机的参考博客：

[verilog之状态机详细解释（一）](https://blog.csdn.net/woshiyuzhoushizhe/article/details/95866063)

[Verilog学习笔记一 状态机](https://www.cnblogs.com/xiaozhu5208/p/12358873.html)

>Moore 型状态机

Moore 型状态机的输出只与当前状态有关，与当前输入无关。

输出会在一个完整的时钟周期内保持稳定，即使此时输入信号有变化，输出也不会变化。输入对输出的影响要到下一个时钟周期才能反映出来。这也是 Moore 型状态机的一个重要特点：`输入与输出是隔离开来的`。

![2021-08-01-nscscc8](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-08-01-nscscc8.png)

>Mealy 型状态机

Mealy 型状态机的输出，不仅与当前状态有关，还取决于当前的输入信号。

Mealy 型状态机的输出是在输入信号变化以后立刻发生变化，且输入变化可能出现在任何状态的时钟周期内。因此，同种逻辑下，Mealy 型状态机输出对输入的响应会比 Moore 型状态机早一个时钟周期。

![2021-08-01-nscscc9](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-08-01-nscscc9.png)

>>一段式状态机

一段式状态机只选择一个状态标志位，这个状态标志位会在输入的决定下选择跳转到下一个状态还是维持原有状态，在每一个状态下检测状态标志位及输入来决定其状态的跳转及输出。其输出和状态的切换在一个always循环块中执行

>>二段式状态机

二段式状态机将状态分为当前状态和次状态，其系统会自动将次状态更新到当前状态，其输入更新在次状态上，其决定系统的状态切换和输出。其输出和状态的切换在两个个always循环块中执行，第一个always块决定系统状态标志的自动跳转，第二个always块决定系统根据不同状态下的输入进行状态的跳转及输出

>>三段式状态机

一般使用最多的是` Mealy 型 3 段式状态机`

其输出和状态更新和状态切换在三个always块中，第一个always块决定系统状态标志的自动跳转，第二个always块决定系统根据不同状态下的输入进行状态的切换，第三个always块根据系统的当前状态决定输出的值。

    (0) 首先，根据状态机的个数确定状态机编码。利用编码给状态寄存器赋值，代码可读性更好。
    (1) 状态机第一段，时序逻辑，非阻塞赋值，传递寄存器的状态。
    (2) 状态机第二段，组合逻辑，阻塞赋值，根据当前状态和当前输入，确定下一个状态机的状态。
    (3) 状态机第三段，时序逻辑，非阻塞赋值，因为是 Mealy 型状态机，根据当前状态和当前输入，确定输出信号。

>实战

菜鸟教程：

<https://www.runoob.com/w3cnote/verilog-fsm.html>

<https://www.cnblogs.com/xianyufpga/p/11006113.html>

[状态机详细解释](https://blog.csdn.net/woshiyuzhoushizhe/article/details/95866063)

[彻底搞懂状态机（一段式、两段式、三段式）](https://blog.csdn.net/wordwarwordwar/article/details/78509445)

>>状态机设计

根据设计需求画出状态转移图，确定使用状态机类型，并标注出各种输入输出信号，更有助于编程。一般使用最多的是 Mealy 型 3 段式状态机，下面用通过设计一个自动售卖机的具体实例来说明状态机的设计过程。

+ 自动售卖机

自动售卖机的功能描述如下：

饮料单价 2 元，该售卖机只能接受 0.5 元、1 元的硬币。考虑找零和出货。投币和出货过程都是一次一次的进行，不会出现一次性投入多币或一次性出货多瓶饮料的现象。每一轮售卖机接受投币、出货、找零完成后，才能进入到新的自动售卖状态。

该售卖机的工作状态转移图如下所示，包含了输入、输出信号状态。

![2021-08-01-nscscc10](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-08-01-nscscc10.png)

其中，coin = 1 代表投入了 0.5 元硬币，coin = 2 代表投入了 1 元硬币。

>状态机设计：3 段式（推荐）

RTL代码：

```verilog
`timescale 1ns / 1ps
// vending-machine
// 2 yuan for a bottle of drink
// only 2 coins supported: 5 jiao and 1 yuan
// finish the function of selling and changing

module  test  (
    input           clk ,//时钟信号
    input           rstn ,//复位信号
    input [1:0]     coin ,     //01 for 0.5 jiao, 10 for 1 yuan

    output [1:0]    change ,//找零
    output          sell    //output the drink 出货
    );

    //machine state decode 根据状态机的个数确定状态机编码。利用编码给状态寄存器赋值
    parameter            IDLE   = 2'd0 ;
    parameter            GET05  = 2'd1 ;
    parameter            GET10  = 2'd2 ;
    parameter            GET15  = 2'd3 ;//设置常量，相当于c++里的define，设置全局常量，在模块中声明后，后续编译时还可以被重新声明的值所覆盖

    //machine variable
    reg [2:0]            st_next ;
    reg [2:0]            st_cur ;

    //(1) state transfer
    //状态转换 时序逻辑，非阻塞赋值，传递寄存器的状态
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            st_cur      <= 'b0 ;
        end
        else begin
            st_cur      <= st_next ;
        end
    end

    //(2) state switch, using block assignment for combination-logic
    //all case items need to be displayed completely    
    //组合逻辑，阻塞赋值，根据当前状态和当前输入，确定下一个状态机的状态
    always @(*) begin
        //st_next = st_cur ;//如果条件选项考虑不全，可以赋初值消除latch
        case(st_cur)
            IDLE:
                case (coin)
                    2'b01:     st_next = GET05 ;
                    2'b10:     st_next = GET10 ;
                    default:   st_next = IDLE ;
                endcase
            GET05:
                case (coin)
                    2'b01:     st_next = GET10 ;
                    2'b10:     st_next = GET15 ;
                    default:   st_next = GET05 ;
                endcase

            GET10:
                case (coin)
                    2'b01:     st_next = GET15 ;
                    2'b10:     st_next = IDLE ;
                    default:   st_next = GET10 ;
                endcase
            GET15:
                case (coin)
                    2'b01,2'b10:
                               st_next = IDLE ;
                    default:   st_next = GET15 ;
                endcase
            default:    st_next = IDLE ;
        endcase
    end

    //(3) output logic, using non-block assignment
    //时序逻辑，非阻塞赋值，因为是 Mealy 型状态机，根据当前状态和当前输入，确定输出信号
    reg  [1:0]   change_r ;//找零
    reg          sell_r ;//出货
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            change_r       <= 2'b0 ;
            sell_r         <= 1'b0 ;
        end
        else if ((st_cur == GET15 && coin ==2'h1)
               || (st_cur == GET10 && coin ==2'd2)) begin
            change_r       <= 2'b0 ;
            sell_r         <= 1'b1 ;
        end
        else if (st_cur == GET15 && coin == 2'h2) begin
            change_r       <= 2'b1 ;
            sell_r         <= 1'b1 ;
        end
        else begin
            change_r       <= 2'b0 ;
            sell_r         <= 1'b0 ;
        end
    end
    assign       sell    = sell_r ;
    assign       change  = change_r ;

endmodule

```

激励文件：

仿真中模拟了 4 种情景，分别是：
```
case1 对应连续输入 4 个 5 角硬币；
case2 对应 1 元 - 5 角 - 1 元的投币顺序；
case3 对应 5 角 - 1 元 - 5 角的投币顺序；
case4 对应连续 3 个 5 角然后一个 1 元的投币顺序。
```


```verilog
`timescale 1ns / 1ps
module one_tb(
    );
reg          clk;
    reg          rstn ;
    reg [1:0]    coin ;//硬币
    wire [1:0]   change ;//找零
    wire         sell ;//出货

    //clock generating
    parameter    CYCLE_200MHz = 10 ; //定义时钟循环信号量，10个时间单位为1个时钟变化循环
    always begin
        clk = 0 ; #(CYCLE_200MHz/2) ;
        clk = 1 ; #(CYCLE_200MHz/2) ;
    end

    //motivation generating
    reg [9:0]    buy_oper ; //store state of the buy operation
    initial begin
        buy_oper  = 'h0 ;
        coin      = 2'h0 ;
        rstn      = 1'b0 ;
        #8 rstn   = 1'b1 ;
        @(negedge clk) ;

        //case(1) 0.5 -> 0.5 -> 0.5 -> 0.5 从右往左计算
        #16 ;
        buy_oper  = 10'b00_0101_0101 ;
        repeat(5) begin
            @(negedge clk) ;
            coin      = buy_oper[1:0] ;
            buy_oper  = buy_oper >> 2 ;
        end

        //case(2) 1 -> 0.5 -> 1, taking change
        #16 ;
        buy_oper  = 10'b00_0010_0110 ;
        repeat(5) begin
            @(negedge clk) ;
            coin      = buy_oper[1:0] ;
            buy_oper  = buy_oper >> 2 ;
        end

        //case(3) 0.5 -> 1 -> 0.5
        #16 ;
        buy_oper  = 10'b00_0001_1001 ;
        repeat(5) begin
            @(negedge clk) ;
            coin      = buy_oper[1:0] ;
            buy_oper  = buy_oper >> 2 ;
        end

        //case(4) 0.5 -> 0.5 -> 0.5 -> 1, taking change
        #16 ;
        buy_oper  = 10'b00_1001_0101 ;
        repeat(5) begin
            @(negedge clk) ;
            coin      = buy_oper[1:0] ;
            buy_oper  = buy_oper >> 2 ;
        end
    end

   //(1) mealy state with 3-stage 例化
    test t0 (
        .clk              (clk),
        .rstn             (rstn),
        .coin             (coin),
        .change           (change),
        .sell             (sell)
        );

   //simulation finish  仿真结束，当时间大于10000个时间单位(ns)时，仿真结束
   always begin
      #100;
      if ($time >= 10000)  $finish ;
   end
endmodule
```

仿真结果如下图所示：

![2021-10-20-sellstation1](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-20-sellstation1.png)

![2021-10-20-sellstation2](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-20-sellstation2.png)

![2021-10-20-sellstation3](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-20-sellstation3.png)

![2021-10-20-sellstation4](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-20-sellstation4.png)



>状态机修改：2 段式

将 3 段式状态机 2、3 段描述合并，其他部分保持不变，状态机就变成了 2 段式描述。

修改部分如下：

```verilog
//(2) state switch, and output logic
//all using block assignment for combination-logic
reg  [1:0]   change_r ;
reg          sell_r ;
always @(*) begin //all case items need to be displayed completely
    case(st_cur)
        IDLE: begin
            change_r     = 2'b0 ;
            sell_r       = 1'b0 ;
            case (coin)
                2'b01:     st_next = GET05 ;
                2'b10:     st_next = GET10 ;
                default:   st_next = IDLE ;
            endcase // case (coin)
        end
        GET05: begin
            change_r     = 2'b0 ;
            sell_r       = 1'b0 ;
            case (coin)
                2'b01:     st_next = GET10 ;
                2'b10:     st_next = GET15 ;
                default:   st_next = GET05 ;
            endcase // case (coin)
        end

        GET10:
            case (coin)
                2'b01:     begin
                    st_next      = GET15 ;
                    change_r     = 2'b0 ;
                    sell_r       = 1'b0 ;
                end
                2'b10:     begin
                    st_next      = IDLE ;
                    change_r     = 2'b0 ;
                    sell_r       = 1'b1 ;
                end
                default:   begin
                    st_next      = GET10 ;
                    change_r     = 2'b0 ;
                    sell_r       = 1'b0 ;
                end
            endcase // case (coin)

        GET15:
            case (coin)
                2'b01: begin
                    st_next     = IDLE ;
                    change_r    = 2'b0 ;
                    sell_r      = 1'b1 ;
                end
                2'b10:     begin
                    st_next     = IDLE ;
                    change_r    = 2'b1 ;
                    sell_r      = 1'b1 ;
                end
                default:   begin
                    st_next     = GET15 ;
                    change_r    = 2'b0 ;
                    sell_r      = 1'b0 ;
                end
            endcase
        default:  begin
            st_next     = IDLE ;
            change_r    = 2'b0 ;
            sell_r      = 1'b0 ;
        end

    endcase
end
```
将上述修改的新模块例化到 3 段式的 testbench 中即可进行仿真

仿真结果如下图所示：




由图可知，出货信号 sell 和 找零信号 change 相对于 3 段式状态机输出提前了一个时钟周期，这是因为输出信号都是阻塞赋值导致的。

如图中红色圆圈部分，输出信号都出现了干扰脉冲，这是因为输入信号都是异步的，而且输出信号是组合逻辑输出，没有时钟驱动。

实际中，如果输入信号都是与时钟同步的，这种干扰脉冲是不会出现的。如果是异步输入信号，首先应当对信号进行同步。

>状态机修改：1 段式（慎用）

将 3 段式状态机 1、 2、3 段描述合并，状态机就变成了 1 段式描述。

```verilog
 //(1) using one state-variable do describe
    reg  [1:0]   change_r ;
    reg          sell_r ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            st_cur     <= 'b0 ;
            change_r   <= 2'b0 ;
            sell_r     <= 1'b0 ;
        end
        else begin
            case(st_cur)

            IDLE: begin
                change_r  <= 2'b0 ;
                sell_r    <= 1'b0 ;
                case (coin)
                    2'b01:     st_cur <= GET05 ;
                    2'b10:     st_cur <= GET10 ;
                endcase
            end
            GET05: begin
                case (coin)
                    2'b01:     st_cur <= GET10 ;
                    2'b10:     st_cur <= GET15 ;
                endcase
            end

            GET10:
                case (coin)
                    2'b01:     st_cur   <=  GET15 ;
                    2'b10:     begin
                        st_cur   <= IDLE ;
                        sell_r   <= 1'b1 ;
                    end
                endcase

            GET15:
                case (coin)
                    2'b01:     begin
                        st_cur   <= IDLE ;
                        sell_r   <= 1'b1 ;
                    end
                    2'b10:     begin
                        st_cur   <= IDLE ;
                        change_r <= 2'b1 ;
                        sell_r   <= 1'b1 ;
                    end
                endcase

            default:  begin
                  st_cur    <= IDLE ;
            end

            endcase // case (st_cur)
        end // else: !if(!rstn)
    end
```
将上述修改的新模块例化到 3 段式的 testbench 中即可进行仿真

仿真结果如下图所示：




1 段式状态机的缺点就是许多种逻辑糅合在一起，不易后期的维护。当状态机和输出信号较少时，可以尝试此种描述方式

>状态机修改：Moore 型

如果使用 Moore 型状态机描述售卖机的工作流程，那么还需要再增加 2 个状态编码，用以描述 Mealy 状态机输出时的`输入信号`和`状态机状态`。

3 段式 Moore 型状态机描述的自动售卖机：

RTL代码：
```verilog
module  vending_machine_moore    (
    input           clk ,
    input           rstn ,
    input [1:0]     coin ,     //01 for 0.5 jiao, 10 for 1 yuan

    output [1:0]    change ,
    output          sell    //output the drink
    );

    //machine state decode
    parameter            IDLE   = 3'd0 ;
    parameter            GET05  = 3'd1 ;
    parameter            GET10  = 3'd2 ;
    parameter            GET15  = 3'd3 ;
    // new state for moore state-machine
    parameter            GET20  = 3'd4 ;
    parameter            GET25  = 3'd5 ;

    //machine variable
    reg [2:0]            st_next ;
    reg [2:0]            st_cur ;

    //(1) state transfer
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            st_cur      <= 'b0 ;
        end
        else begin
            st_cur      <= st_next ;
        end
    end

    //(2) state switch, using block assignment for combination-logic
    always @(*) begin //all case items need to be displayed completely
        case(st_cur)
            IDLE:
                case (coin)
                    2'b01:     st_next = GET05 ;
                    2'b10:     st_next = GET10 ;
                    default:   st_next = IDLE ;
                endcase
            GET05:
                case (coin)
                    2'b01:     st_next = GET10 ;
                    2'b10:     st_next = GET15 ;
                    default:   st_next = GET05 ;
                endcase

            GET10:
                case (coin)
                    2'b01:     st_next = GET15 ;
                    2'b10:     st_next = GET20 ;
                    default:   st_next = GET10 ;
                endcase
            GET15:
                case (coin)
                    2'b01:     st_next = GET20 ;
                    2'b10:     st_next = GET25 ;
                    default:   st_next = GET15 ;
                endcase
            GET20:         st_next = IDLE ;
            GET25:         st_next = IDLE ;
            default:       st_next = IDLE ;
        endcase // case (st_cur)
    end // always @ (*)

   // (3) output logic,
   // one cycle delayed when using non-block assignment
    reg  [1:0]   change_r ;
    reg          sell_r ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            change_r       <= 2'b0 ;
            sell_r         <= 1'b0 ;
        end
        else if (st_cur == GET20 ) begin
            sell_r         <= 1'b1 ;
        end
        else if (st_cur == GET25) begin
            change_r       <= 2'b1 ;
            sell_r         <= 1'b1 ;
        end
        else begin
            change_r       <= 2'b0 ;
            sell_r         <= 1'b0 ;
        end
    end
    assign       sell    = sell_r ;
    assign       change  = change_r ;

endmodule
```
将上述修改的 Moore 状态机例化到 3 段式的 testbench 中即可进行仿真

仿真结果如下图所示：




由图可知，输出信号与 Mealy 型 3 段式状态机相比延迟了一个时钟周期，这是因为进入到新增加的编码状态机时需要一个时钟周期的时延。此时，输出再用非阻塞赋值就会导致最终的输出信号延迟一个时钟周期。这也属于 Moore 型状态机的特点。

***

输出信号赋值时，用阻塞赋值，则可以提前一个时钟周期。

输出逻辑修改如下：

```verilog
 // (3.2) output logic, using block assignment
    reg  [1:0]   change_r ;
    reg          sell_r ;
    always @(*) begin
        change_r  = 'b0 ;
        sell_r    = 'b0 ; //not list all condition, initializing them
        if (st_cur == GET20 ) begin
            sell_r         = 1'b1 ;
        end
        else if (st_cur == GET25) begin
            change_r       = 2'b1 ;
            sell_r         = 1'b1 ;
        end
    end
```
`输出信号阻塞赋值`的仿真结果如下图所示：





由图可知，输出信号已经和 3 段式 Mealy 型状态机一致。



+ 电梯

描述电梯的四种状态：初始状态idle——电梯开门状态、向上up、向下down、停止stop

最简单的电梯状态机(通过一个信号对四种状态切换)——from chs

RTL代码：

```verilog
`timescale 1ns / 1ps
module test(
    input wire clk,
    input wire rst,
    input wire in,
    output reg [1:0] k
);
    
    parameter idle = 2'b00 ;
    parameter up = 2'b01 ;
    parameter down = 2'b10;
    parameter stop = 2'b11 ;

    reg [1:0] state;
    reg [1:0] next_state;
    
    always @(posedge clk or negedge rst) begin
        if(!rst) begin
            state<=idle;
        end else begin
            state<=next_state;
        end
    end

    always @(*) begin
        next_state = idle;
        case (state)
            idle : begin
                if(in) begin
                    next_state = up;
                end else begin
                    next_state = idle;
                end
            end
            up : begin
                if(in) begin
                    next_state = down;
                end else begin
                    next_state = up;
                end
            end
            down : begin
                if(in) begin
                    next_state = stop;
                end else begin
                    next_state = down;
                end
            end
            stop : begin
                if(in) begin
                    next_state = idle;
                end else begin
                    next_state = stop;
                end
            end
            default : next_state = 2'bxx;
        endcase
    end

    always @(*) begin
        case (state)
            idle : begin
                k=idle;
            end
            up : begin
                k=up;
            end
            down : begin
                k=down;
            end
            stop : begin
                k=stop;
            end
            default : begin
                k=2'bxx;
            end
        endcase
    end
endmodule
```

激励文件：
```verilog
`timescale 1ns / 1ps
module test_tb(

    );
    reg clk;
    reg rst;
    reg in;
    wire [1:0] k;
    test t0(.clk(clk),.rst(rst),.in(in),.k(k));
    
    always #2 clk=~clk;
    always #2 in=~in;
    initial begin
        clk=0;
        rst=0;
        in=1;
        #2 rst=1;
    end
endmodule
```

仿真结果如下图所示：

![2021-10-20-station1](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-20-station1.png)






***

## UART串口

参考：

[认识UART接口](https://blog.csdn.net/weixin_38233274/article/details/81105114)

[uart接口定义详解介绍（基本结构及工作原理）](http://www.elecfans.com/pld/jiekou_zongxian_qudong/596884.html)

[UART串口收发的原理与Verilog实现](https://www.cnblogs.com/liujinggang/p/9535366.html)



UART有4个pin（VCC, GND, RX, TX）, 用的TTL电平,  低电平为0(0V)，高电平为1（3.3V或以上）。

COM口是我们台式机上面常用的口，9个pin， 用的RS232电平,  它是负逻辑电平，它定义`+5~+12V`为低电平，而`-12~-5V`为高电平

其中，UART,COM指的物理接口形式(硬件)，TL、RS-232是指的电平标准(电信号)

> 引脚介绍

+ **VCC：**供电pin，一般是3.3v，在我们的板子上没有过电保护，这个pin一般不接更安全

+ **GND：**接地pin，有的时候rx接受数据有问题，就要接上这个pin，一般也可不接

+ **RX：**接收数据pin

+ **TX：**发送数据pin，之前碰到串口只能收数据，不能发数据问题，经baidu，原来是设置了流控制，取消就可以了，适用于putty,SecureCRT

> UART通信协议

UART用一条传输线将数据一位位地顺序传送，以字符为传输单位，通信中两个字符间的时间间隔多少是不固定的， 然而在同一个字符中的两个相邻位间的时间间隔是固定的

数据传送速率用波特率来表示， 指单位时间内载波参数变化的次数， 或每秒钟传送的二进制位数

+ 波特率
+ 比特率

串口传输的速度用波特率(baudrate)来指定。波特率表示的是每秒发送的比特数，单位是bps(bits-per-seconds)，例如，1000 bauds表示1秒钟发送了1000个比特，或者说每个比特持续的时间是1ms

常用的波特率标准有：

1. 1200 bps
2. 9600 bps (常用)
3. 38400 bps
4. 115200 bps (常用，而且通常情况下是我们能用的最快的波特率)





![2021-10-27-uart1](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-27-uart1.png)

其中各位的意义如下：

- 起始位（Start Bit）：先发出一个逻辑0信号， 表示传输字符的开始
- 数据位（Data Bits）：可以是5~8位逻辑0或1. 如ASCII码（7位）， 扩展BCD码（8位）<font color=FF0000>小端传输</font>
- 校验位（Parity Bit）：数据位加上这一位后， 使得1的位数应为偶数（偶校验）或奇数（奇校验）
- 停止位（Stop Bit）：它是一个字符数据的结束标志。 可以是1位、1.5位、2位的高电平
- 空闲位：处于逻辑1状态， 表示当前线路上没有资料传送

> UART基本结构

⑴ 输出缓冲寄存器，它接收[CPU](http://www.elecfans.com/tags/cpu/)从数据总线上送来的并行数据，并加以保存。

⑵ 输出移位[寄存器](http://www.elecfans.com/tags/寄存器/)，它接收从输出缓冲器送来的并行数据，以发送时钟的速率把数据逐位移出，即将并行数据转换为串行数据输出。

⑶ 输入移位寄存器，它以接收时钟的速率把出现在串行数据输入线上的数据逐位移入，当数据装满后，并行送往输入缓冲寄存器，即 将串行数据转换成并行数据。

⑷ 输入缓冲寄存器，它从输入移位寄存器中接收并行数据，然后由CPU取走。

⑸ 控制寄存器，它接收CPU送来的控制字，由控制字的内容，决定通信时的传输方式以及数据格式等。例如采用异步方式还是同步方式，数据字符的位数，有无奇偶校验，是奇校验还是偶校验，停止位的位数等参数。

⑹ 状态寄存器。状态寄存器中存放着接口的各种状态信息，例如输出缓冲区是否空，输入字符是否准备好等。在通信过程中，当符合某种状态时，接口中的状态检测逻辑将状态寄存器的相应位置“1”，以便让CPU查询。

![2021-10-27-uart2](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-27-uart2.png)

> UART工作原理

发送数据过程：空闲状态，线路处于高电位；当收到发送数据指令后，拉低线路一个数据位的时间T，接着数据按低位到高位依次发送，数据发送完毕后，接着发送奇偶校验位和停止位（停止位为高电位），一帧数据发送结束。

接收数据过程：空闲状态，线路处于高电位；当检测到线路的下降沿（线路电位由高电位变为低电位）时说明线路有数据传输，按照约定的波特率从低位到高位接收数据，数据接收完毕后，接着接收并比较奇偶校验位是否正确，如果正确则通知后续设备准备接收数据或存入缓存。



FPGA与PC之间的串口通信主要包括三个模块：波特率产生模块、发射模块和接收模块。



RTL代码：

![2021-10-27-uart3](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-27-uart3.png)

其中：

　　I_clk是系统时钟；

　　I_rst_n是系统复位；

　　I_tx_bps_en是发射模块波特率使能信号，当I_tx_bps_en为1时O_bps_tx_clk才有时钟信号输出；

　　I_rx_bps_en是接收模块波特率使能信号，当I_rx_bps_en为1时O_bps_rx_clk才有时钟信号输出。

波特率模块的完整代码如下：

```verilog
`timescale 1ns / 1ps

module baudrate_gen(
    input   I_clk                  , // 系统50MHz时钟
    input   I_rst_n                , // 系统全局复位
    input   I_bps_tx_clk_en        , // 串口发送模块波特率时钟使能信号
    input   I_bps_rx_clk_en        , // 串口接收模块波特率时钟使能信号
    output  O_bps_tx_clk           , // 发送模块波特率产生时钟
    output  O_bps_rx_clk             // 接收模块波特率产生时钟
);

parameter       C_BPS9600         = 5207         ,    //波特率为9600bps
                C_BPS19200        = 2603         ,    //波特率为19200bps
                C_BPS38400        = 1301         ,    //波特率为38400bps
                C_BPS57600        = 867          ,    //波特率为57600bps
                C_BPS115200       = 433          ;    //波特率为115200bps
                
parameter       C_BPS_SELECT      = C_BPS115200  ;    //波特率选择
                

reg [12:0]  R_bps_tx_cnt       ;
reg [12:0]  R_bps_rx_cnt       ;

///////////////////////////////////////////////////////////    
// 功能：串口发送模块的波特率时钟产生逻辑
///////////////////////////////////////////////////////////
always @(posedge I_clk or negedge I_rst_n)
begin
    if(!I_rst_n)
        R_bps_tx_cnt <= 13'd0 ;
    else if(I_bps_tx_clk_en == 1'b1)
        begin
            if(R_bps_tx_cnt == C_BPS_SELECT)
                R_bps_tx_cnt <= 13'd0 ;
            else
                R_bps_tx_cnt <= R_bps_tx_cnt + 1'b1 ;                 
        end    
    else
        R_bps_tx_cnt <= 13'd0 ;        
end

assign O_bps_tx_clk = (R_bps_tx_cnt == 13'd1) ? 1'b1 : 1'b0 ;

///////////////////////////////////////////////////////////    
// 功能：串口接收模块的波特率时钟产生逻辑
///////////////////////////////////////////////////////////
always @(posedge I_clk or negedge I_rst_n)
begin
    if(!I_rst_n)
        R_bps_rx_cnt <= 13'd0 ;
    else if(I_bps_rx_clk_en == 1'b1)
        begin
            if(R_bps_rx_cnt == C_BPS_SELECT)
                R_bps_rx_cnt <= 13'd0 ;
            else
                R_bps_rx_cnt <= R_bps_rx_cnt + 1'b1 ;                 
        end    
    else
        R_bps_rx_cnt <= 13'd0 ;        
end

assign O_bps_rx_clk = (R_bps_rx_cnt == C_BPS_SELECT >> 1'b1) ? 1'b1 : 1'b0 ;

endmodule
```

![2021-10-27-uart4](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-27-uart4.png)

 其中：

　　I_clk是系统时钟；

　　I_rst_n是系统复位；

　　I_tx_start是开始发送信号，当检测到I_tx_start为高电平时，立马把输入I_para_data[7:0]的数据串行化成单bit的发出去；

　　I_bps_tx_clk是发送模块波特率时钟信号，当检测到I_bps_tx_clk为高的时候就发送1个bit；

　　I_para_data[7:0]是并行的8-bit数据；

　　O_rs232_txd是串行的bit数据流；

　　O_bps_clk_en是发射波特率时钟启动信号，当它为1是波特率产生模块才能产生发射模块的波特率时钟；

　　O_tx_done是发送1字节数据完成的标志位，当一个字节发送完毕以后，O_tx_done产生一个高脉冲。

发送模块的代码如下：

```verilog
`timescale 1ns / 1ps

module uart_txd(
    input          I_clk           , // 系统50MHz时钟
    input          I_rst_n         , // 系统全局复位
    input          I_tx_start      , // 发送使能信号
    input          I_bps_tx_clk    , // 发送波特率时钟
    input   [7:0]  I_para_data     , // 要发送的并行数据
    output  reg    O_rs232_txd     , // 发送的串行数据，在硬件上与串口相连
    output  reg    O_bps_tx_clk_en , // 波特率时钟使能信号
    output  reg    O_tx_done         // 发送完成的标志
);

reg  [3:0]  R_state ;

reg R_transmiting ; // 数据正在发送标志

/////////////////////////////////////////////////////////////////////////////
// 产生发送 R_transmiting 标志位
/////////////////////////////////////////////////////////////////////////////
always @(posedge I_clk or negedge I_rst_n)
begin
    if(!I_rst_n)
        R_transmiting <= 1'b0 ;
    else if(O_tx_done)
        R_transmiting <= 1'b0 ;
    else if(I_tx_start)
        R_transmiting <= 1'b1 ;          
end

/////////////////////////////////////////////////////////////////////////////
// 发送数据状态机
/////////////////////////////////////////////////////////////////////////////
always @(posedge I_clk or negedge I_rst_n)
begin
    if(!I_rst_n)
        begin
            R_state      <= 4'd0 ;
            O_rs232_txd  <= 1'b1 ; 
            O_tx_done    <= 1'b0 ;
            O_bps_tx_clk_en <= 1'b0 ;  // 关掉波特率时钟使能信号
        end 
    else if(R_transmiting) // 检测发送标志被拉高，准备发送数据
        begin
            O_bps_tx_clk_en <= 1'b1 ;  // 发送数据前的第一件事就是打开波特率时钟使能信号
            if(I_bps_tx_clk) // 在波特率时钟的控制下把数据通过一个状态机发送出去，并产生发送完成信号
                begin
                    case(R_state)
                        4'd0  : // 发送起始位
                            begin
                                O_rs232_txd  <= 1'b0            ;
                                O_tx_done    <= 1'b0            ; 
                                R_state      <= R_state + 1'b1  ;
                            end
                        4'd1  : // 发送 I_para_data[0]
                            begin
                                O_rs232_txd  <= I_para_data[0]  ;
                                O_tx_done    <= 1'b0            ; 
                                R_state      <= R_state + 1'b1  ;
                            end 
                        4'd2  : // 发送 I_para_data[1]
                            begin
                                O_rs232_txd  <= I_para_data[1]   ;
                                O_tx_done    <= 1'b0             ; 
                                R_state      <= R_state + 1'b1   ;
                            end
                        4'd3  : // 发送 I_para_data[2]
                            begin
                                O_rs232_txd  <= I_para_data[2]   ;
                                O_tx_done    <= 1'b0             ; 
                                R_state      <= R_state + 1'b1   ;
                            end
                        4'd4  : // 发送 I_para_data[3]
                            begin
                                O_rs232_txd  <= I_para_data[3]   ;
                                O_tx_done    <= 1'b0             ; 
                                R_state      <= R_state + 1'b1   ;
                            end 
                        4'd5  : // 发送 I_para_data[4]
                            begin
                                O_rs232_txd  <= I_para_data[4]   ;
                                O_tx_done    <= 1'b0             ; 
                                R_state      <= R_state + 1'b1   ;
                            end
                        4'd6  : // 发送 I_para_data[5]
                            begin
                                O_rs232_txd  <= I_para_data[5]   ;
                                O_tx_done    <= 1'b0             ; 
                                R_state      <= R_state + 1'b1   ;
                            end
                        4'd7  : // 发送 I_para_data[6]
                            begin
                                O_rs232_txd  <= I_para_data[6]   ;
                                O_tx_done    <= 1'b0             ; 
                                R_state      <= R_state + 1'b1   ;
                            end
                        4'd8  : // 发送 I_para_data[7]
                            begin
                                O_rs232_txd  <= I_para_data[7]   ;
                                O_tx_done    <= 1'b0             ; 
                                R_state      <= R_state + 1'b1   ;
                            end 
                        4'd9  : // 发送 停止位
                            begin
                                O_rs232_txd  <= 1'b1             ;
                                O_tx_done    <= 1'b1             ; 
                                R_state      <= 4'd0             ;
                            end
                        default :R_state      <= 4'd0            ;

                    endcase 
                end 
            end 
        else 
            begin 
                O_bps_tx_clk_en     <= 1'b0  ; // 一帧数据发送完毕以后就关掉波特率时钟使能信号 
                R_state             <= 4'd0  ; 
                O_tx_done           <= 1'b0  ; 
                O_rs232_txd         <= 1'b1  ;  
            end 
end
endmodule

```

![2021-10-27-uart5](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-27-uart5.png)

其中：

　　I_clk是系统时钟；

　　I_rst_n是系统复位；

　　I_rx_start是开始发送信号，当I_rx_start一直为高电平时，接收模块检测到有数据就会接收；

　　I_bps_rx_clk是接收模块波特率时钟信号，当检测到I_bps_rx_clk为高的时候就接收1个bit；

　　I_rs232_rx是串行的bit数据流；

　　O_para_data[7:0]是并行的8-bit数据；

　　O_bps_rx_clk_en是发射波特率时钟启动信号，当它为1是波特率产生模块才能产生接收模块的波特率时钟；

　　O_rx_done是接收1字节数据完成的标志位，当一个字节接收完毕以后，O_rx_done产生一个高脉冲。

接收模块的代码如下：

```verilog
`timescale 1ns / 1ps
module uart_rxd(
    input                       I_clk               , // 系统50MHz时钟
    input                       I_rst_n             , // 系统全局复位
    input                       I_rx_start          , // 接收使能信号
    input                       I_bps_rx_clk        , // 接收波特率时钟
    input                       I_rs232_rxd         , // 接收的串行数据，在硬件上与串口相连  
    output    reg               O_bps_rx_clk_en     , // 波特率时钟使能信号
    output    reg               O_rx_done           , // 接收完成标志
    output    reg   [7:0]       O_para_data           // 接收到的8-bit并行数据
);

reg         R_rs232_rx_reg0 ;
reg         R_rs232_rx_reg1 ;
reg         R_rs232_rx_reg2 ;
reg         R_rs232_rx_reg3 ;

reg         R_receiving     ;

reg [3:0]   R_state         ;
reg [7:0]   R_para_data_reg ;

wire        W_rs232_rxd_neg ;

////////////////////////////////////////////////////////////////////////////////
// 功能：把 I_rs232_rxd 打的前两拍，是为了消除亚稳态
//          把 I_rs232_rxd 打的后两拍，是为了产生下降沿标志位
////////////////////////////////////////////////////////////////////////////////
always @(posedge I_clk or negedge I_rst_n)
begin
    if(!I_rst_n)
        begin
            R_rs232_rx_reg0 <= 1'b0 ;
            R_rs232_rx_reg1 <= 1'b0 ;
            R_rs232_rx_reg2 <= 1'b0 ;
            R_rs232_rx_reg3 <= 1'b0 ;
        end 
    else
        begin  
            R_rs232_rx_reg0 <= I_rs232_rxd      ;
            R_rs232_rx_reg1 <= R_rs232_rx_reg0  ; 
            R_rs232_rx_reg2 <= R_rs232_rx_reg1  ; 
            R_rs232_rx_reg3 <= R_rs232_rx_reg2  ; 
        end   
end
// 产生I_rs232_rxd信号的下降沿标志位
assign W_rs232_rxd_neg    =    (~R_rs232_rx_reg2) & R_rs232_rx_reg3 ;

////////////////////////////////////////////////////////////////////////////////
// 功能：产生发送信号R_receiving
////////////////////////////////////////////////////////////////////////////////
always @(posedge I_clk or negedge I_rst_n)
begin
    if(!I_rst_n)
        R_receiving <= 1'b0 ;
    else if(O_rx_done)
        R_receiving <= 1'b0 ;
    else if(I_rx_start && W_rs232_rxd_neg)
        R_receiving <= 1'b1 ;          
end

////////////////////////////////////////////////////////////////////////////////
// 功能：用状态机把串行的输入数据接收，并转化为并行数据输出
////////////////////////////////////////////////////////////////////////////////
always @(posedge I_clk or negedge I_rst_n)
begin
    if(!I_rst_n)
        begin
            O_rx_done       <= 1'b0 ; 
            R_state         <= 4'd0 ;
            R_para_data_reg <= 8'd0 ;
            O_bps_rx_clk_en <= 1'b0 ;
        end 
    else if(R_receiving)
        begin
            O_bps_rx_clk_en <= 1'b1 ; // 打开波特率时钟使能信号
            if(I_bps_rx_clk)
                begin
                    case(R_state)
                        4'd0  : // 接收起始位，但不保存
                            begin
                                R_para_data_reg     <= 8'd0             ;
                                O_rx_done           <= 1'b0             ; 
                                R_state             <= R_state + 1'b1   ;
                            end
                        4'd1  : // 接收第0位，保存到R_para_data_reg[0]
                            begin
                                R_para_data_reg[0]  <= I_rs232_rxd      ;
                                O_rx_done           <= 1'b0             ; 
                                R_state             <= R_state + 1'b1   ;
                            end
                        4'd2  : // 接收第1位，保存到R_para_data_reg[1]
                            begin
                                R_para_data_reg[1]  <= I_rs232_rxd      ;
                                O_rx_done           <= 1'b0             ; 
                                R_state             <= R_state + 1'b1   ;
                            end
                        4'd3  : // 接收第2位，保存到R_para_data_reg[2]
                            begin
                                R_para_data_reg[2]  <= I_rs232_rxd      ;
                                O_rx_done           <= 1'b0             ; 
                                R_state             <= R_state + 1'b1   ;
                            end 
                        4'd4  : // 接收第3位，保存到R_para_data_reg[3]
                            begin
                                R_para_data_reg[3]  <= I_rs232_rxd      ;
                                O_rx_done           <= 1'b0             ; 
                                R_state             <= R_state + 1'b1   ;
                            end 
                        4'd5  : // 接收第4位，保存到R_para_data_reg[4]
                            begin
                                R_para_data_reg[4]  <= I_rs232_rxd      ;
                                O_rx_done           <= 1'b0             ; 
                                R_state             <= R_state + 1'b1   ;
                            end
                        4'd6  : // 接收第5位，保存到R_para_data_reg[5]
                            begin
                                R_para_data_reg[5]  <= I_rs232_rxd      ;
                                O_rx_done           <= 1'b0             ; 
                                R_state             <= R_state + 1'b1   ;
                            end
                        4'd7  :// 接收第6位，保存到R_para_data_reg[6]
                            begin
                                R_para_data_reg[6]  <= I_rs232_rxd      ;
                                O_rx_done           <= 1'b0             ; 
                                R_state             <= R_state + 1'b1   ;
                            end
                        4'd8  : // 接收第7位，保存到R_para_data_reg[7]
                            begin
                                R_para_data_reg[7]  <= I_rs232_rxd      ;
                                O_rx_done           <= 1'b0             ; 
                                R_state             <= R_state + 1'b1   ;
                            end 
                        4'd9  : // 接收停止位，但不保存，并把R_para_data_reg给输出
                            begin
                                O_para_data         <= R_para_data_reg  ;
                                O_rx_done           <= 1'b1             ; 
                                R_state             <= 4'd0             ;
                            end 
                            
                        default:R_state <= 4'd0                         ;                                                      
                    endcase 
                end
        end
    else
        begin
            O_rx_done           <= 1'b0 ;
            R_state             <= 4'd0 ;
            R_para_data_reg     <= 8'd0 ;
            O_bps_rx_clk_en     <= 1'b0 ; // 接收完毕以后关闭波特率时钟使能信号
        end          
end

endmodule
```





顶层文件代码【内嵌激励文件】：

```verilog
`timescale 1ns / 1ps

module uart_top
(
    input            I_clk           , // 系统50MHz时钟
    input            I_rst_n         , // 系统全局复位
    input            I_rs232_rxd     , // 接收的串行数据，在硬件上与串口相连
    output           O_rs232_txd     , // 发送的串行数据，在硬件上与串口相连
    output    [3:0]  O_led_out       
);

wire            W_bps_tx_clk                 ;
wire            W_bps_tx_clk_en              ;
wire            W_bps_rx_clk                 ;
wire            W_bps_rx_clk_en              ;
wire            W_rx_done                    ;
wire            W_tx_done                    ;
wire  [7:0]     W_para_data                  ;

assign    O_led_out = W_para_data[3:0]       ;

baudrate_gen U_baudrate_gen
(
    .I_clk              (I_clk              ), // 系统50MHz时钟
    .I_rst_n            (I_rst_n            ), // 系统全局复位
    .I_bps_tx_clk_en    (W_bps_tx_clk_en    ), // 串口发送模块波特率时钟使能信号
    .I_bps_rx_clk_en    (W_bps_rx_clk_en    ), // 串口接收模块波特率时钟使能信号
    .O_bps_tx_clk       (W_bps_tx_clk       ), // 发送模块波特率产生时钟
    .O_bps_rx_clk       (W_bps_rx_clk       )  // 接收模块波特率产生时钟
);

uart_txd U_uart_txd
(
    .I_clk               (I_clk                 ), // 系统50MHz时钟
    .I_rst_n             (I_rst_n               ), // 系统全局复位
    .I_tx_start          (W_rx_done             ), // 发送使能信号
    .I_bps_tx_clk        (W_bps_tx_clk          ), // 波特率时钟
    .I_para_data         (W_para_data           ), // 要发送的并行数据
    .O_rs232_txd         (O_rs232_txd           ), // 发送的串行数据，在硬件上与串口相连
    .O_bps_tx_clk_en     (W_bps_tx_clk_en       ), // 波特率时钟使能信号
    .O_tx_done           (W_tx_done             )  // 发送完成的标志
);

uart_rxd U_uart_rxd
(
    .I_clk              (I_clk                ), // 系统50MHz时钟
    .I_rst_n            (I_rst_n              ), // 系统全局复位
    .I_rx_start         (1'b1                 ), // 接收使能信号
    .I_bps_rx_clk       (W_bps_rx_clk         ), // 接收波特率时钟
    .I_rs232_rxd        (I_rs232_rxd          ), // 接收的串行数据，在硬件上与串口相连  
    .O_bps_rx_clk_en    (W_bps_rx_clk_en      ), // 波特率时钟使能信号
    .O_rx_done          (W_rx_done            ), // 接收完成标志
    .O_para_data        (W_para_data          )  // 接收到的8-bit并行数据
);

endmodule
```



测试发送模块的顶层文件：

```verilog
`timescale 1ns / 1ps

module uart_tx_top
(
    input          I_clk           , // 系统50MHz时钟
    input          I_rst_n         , // 系统全局复位
    output        O_rs232_txd       // 发送的串行数据，在硬件上与串口相连
);

wire            W_bps_tx_clk             ;
wire            W_bps_tx_clk_en          ;
wire            W_tx_start               ;
wire            W_tx_done                ;
wire  [7:0]     W_para_data              ;
        
reg   [7:0]     R_data_reg               ;
reg   [31:0]    R_cnt_1s                 ;
reg             R_tx_start_reg           ;

assign W_tx_start     =    R_tx_start_reg    ;
assign W_para_data    =    R_data_reg        ;

/////////////////////////////////////////////////////////////////////
// 产生要发送的数据
/////////////////////////////////////////////////////////////////////
always @(posedge I_clk or negedge I_rst_n)
begin
     if(!I_rst_n)
        begin
             R_cnt_1s         <= 31'd0     ;
             R_data_reg       <= 8'd0      ;
             R_tx_start_reg   <= 1'b0      ;
        end
     else if(R_cnt_1s == 31'd24_999_999)
        begin
             R_cnt_1s         <= 31'd0                 ;
             R_data_reg       <= R_data_reg + 1'b1      ;
             R_tx_start_reg <= 1'b1                 ;
        end
     else
        begin
          R_cnt_1s             <= R_cnt_1s + 1'b1     ;
          R_tx_start_reg     <= 1'b0             ;
        end
end

uart_txd U_uart_txd
(
    .I_clk               (I_clk                 ), // 系统50MHz时钟
    .I_rst_n             (I_rst_n               ), // 系统全局复位
    .I_tx_start          (W_tx_start            ), // 发送使能信号
    .I_bps_tx_clk        (W_bps_tx_clk          ), // 波特率时钟
    .I_para_data         (W_para_data           ), // 要发送的并行数据
    .O_rs232_txd         (O_rs232_txd           ), // 发送的串行数据，在硬件上与串口相连
    .O_bps_tx_clk_en     (W_bps_tx_clk_en       ), // 波特率时钟使能信号
    .O_tx_done           (W_tx_done             )  // 发送完成的标志
);

baudrate_gen U_baudrate_gen
(
    .I_clk              (I_clk              ), // 系统50MHz时钟
    .I_rst_n            (I_rst_n            ), // 系统全局复位
    .I_bps_tx_clk_en    (W_bps_tx_clk_en    ), // 串口发送模块波特率时钟使能信号
    .I_bps_rx_clk_en    (                   ), // 串口接收模块波特率时钟使能信号
    .O_bps_tx_clk       (W_bps_tx_clk       ), // 发送模块波特率产生时钟
    .O_bps_rx_clk       (                   )  // 接收模块波特率产生时钟
);

endmodule
```



仿真结果如下图所示：

【仅靠vivado跑不出来，可能需要串口助手maybe，待补充~】





# 自己动手写CPU

## 程序计数器 pc

寄存器软件无法直接访问，相当于CS代码段和IP指针

RTL代码：

```verilog
`timescale 1ns / 1ps
module pc(
    input wire rst,
    input wire clk,
    output reg[31:0] pc,
    output reg ce
    );

    always @(posedge clk) begin
        if(rst==1)begin
            ce<=0;
        end else begin
            ce<=1;
        end
    end

    always @(posedge clk) begin
        if(ce==0)begin
            pc<=32'h0;
        end else begin
            pc<=pc+32'h4;
        end
    end
endmodule
```



激励文件：

```verilog
`timescale 1ns / 1ps
module test_tb(

    );
    reg clk;
    reg rst;
    wire ce;
    wire [31:0] pc;
    
    pc pc0(.clk(clk),.rst(rst),.ce(ce),.pc(pc));

    initial begin
        clk<=1;
        #10
        rst<=1;
        #100
        rst<=0;
    end

    always begin
        #10
        clk<=~clk;
    end
endmodule
```





仿真结果如下图所示：

![2021-10-21-pc1](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-21-pc1.png)

![2021-10-21-pc2](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-21-pc2.png)



## MIPS寄存器堆 regfile

写——同步写

+ 写使能信号（we）为1时有效，为0时无效

读——异步读

（1）当复位信号（`rst==1`）有效时，读数据（rdata1和rdata2）为0；

（2）否则当复位信号（`rst==0`）无效时,当读地址为0，读数据为0；

（3）否则当读写地址相等，且读写使能都有效的时候，读数据为写数据；【数据相关：读后写】

（4）否则当读使能有效时，读数据为寄存器堆中存储数据；

（5）其余情况，读数据为0。



RTL代码：

```verilog
`timescale 1ns / 1ps
module regfile(
    input wire rst,
    input wire clk,
    input wire [4:0] waddr,
    input wire [31:0] wdata,
    input wire we,
    input wire [4:0] raddr1,
    input wire re1,
    output reg [31:0] rdata1,
    input wire [4:0] raddr2,
    input wire re2,
    output reg [31:0] rdata2
    );

    reg [31:0] regs[0:31];
    always @(posedge clk) begin
        if(rst==1'b0)begin
            if((we==1'b1)&&(waddr!=5'b0))begin
                regs[waddr]<=wdata;
            end
        end
    end

    always @(*) begin
        if(rst==1)begin
            rdata1<=32'h0;
        end else if((raddr1==5'b0)&&(re1==1'b1))begin
            rdata1<=32'h0;
        end else if((raddr1==waddr)&&(we==1'b1)&&(re1==1'b1))begin
            rdata1<=wdata;
        end else if(re1==1'b1)begin
            rdata1<=regs[raddr1];
        end else begin
            rdata1<=32'h0;
        end
    end

    always @(*) begin
        if(rst==1)begin
            rdata2<=32'h0;
            //当复位信号（rst==1）有效时，读数据（rdata1和rdata2）为0
        end else if((raddr2==5'b0)&&(re2==1'b1))begin
            rdata2<=32'h0;
            //当复位信号（rst==0）无效时,当读地址为0，读数据为0
        end else if((raddr2==waddr)&&(we==1'b1)&&(re2==1'b1))begin
            rdata2<=wdata;
            //当读写地址相等，且读写使能都有效的时候，读数据为写数据【数据相关：写后读】read after write
        end else if(re2==1'b1)begin
            rdata2<=regs[raddr2];
            //当读使能有效时，读数据为寄存器堆中存储数据
        end else begin
            rdata2<=32'h0;
        end
    end
    
endmodule
```



激励文件：

```verilog
`timescale 1ns / 1ps
module test_tb(

    );
    reg clk;
    reg rst;
    reg [4:0] waddr;
    reg [31:0] wdata;
    reg we;
    reg [4:0] raddr1;
    reg re1;
    wire [31:0] rdata1;
    reg [4:0] raddr2;
    reg re2;
    wire [31:0] rdata2;
    
    integer i;
    regfile regfile0(.clk(clk),.rst(rst),.waddr(waddr),.wdata(wdata),.we(we),
                        .re1(re1),.raddr1(raddr1),.rdata1(rdata1),
                        .re2(re2),.raddr2(raddr2),.rdata2(rdata2));

    initial begin
        clk<=1;
        rst<=0;
        $monitor($time,"raddr1=%h,rdata1=%h,raddr2=%h,rdata2=%h",raddr1,rdata1,raddr2,rdata2);
        #50
        rst<=1;
        #50
        rst<=0;
        we<=1;
        for(i=0;i<32;i=i+1)begin
            waddr<=i;
            wdata<=i;
            #30;
        end
        we<=0;
        re1<=1;
        re2<=1;
        for(i=0;i<32;i=i+1)begin
            raddr1<=i;
            raddr2<=32-i;
            #30;
        end
        re1<=0;
        re2<=0;
        #30
        we<=1;
        waddr<=15;
        wdata<=32'h0af;
        re1<=1;
        raddr1<=15;
        #30
        $stop;
    end

    always #10 clk<=~clk;

endmodule
```





仿真结果如下图所示：



![2021-10-21-regfile1](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-21-regfile1.png)



![2021-10-21-regfile2](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-21-regfile2.png)



![2021-10-21-regfile3](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-21-regfile3.png)





## 指令存储器 inst_rom



![2021-10-27-inst_rom3](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-27-inst_rom3.png)

0011

0010

0001

0000

指令存储器按照字进行寻址，将4B变为1个字，即直接舍弃`后两位`

18：2 ，从2开始是因为cpu按字节寻址，但是Inst是一个字，所以后两位一定为00。

131072 为2的17次方，只用地址addr的17：0就行，表示17位二进制数能够表示的地址大小为$2^{17}$

RTL代码：

```verilog
`timescale 1ns / 1ps
module inst_rom(
    input wire ce,
    input wire [31:0] addr,
    output reg [31:0] inst
    );

    reg [31:0] inst_mem[0:131071];
    initial $readmemh("D:/language/Verilog/test3/test3.srcs/sources_1/new/inst_rom.data",inst_mem);
    always @(*) begin
        if(ce==1'b0)begin
            inst<=32'b0;
        end else begin
            inst<=inst_mem[addr[18:2]];
        end
    end
    
endmodule
```



激励文件：

```verilog
`timescale 1ns / 1ps
module test_tb(

    );
    
    reg clk;
    reg ce;
    reg [31:0] addr;
    wire [31:0] inst;
    integer i;

    inst_rom inst_rom0(.ce(ce),.addr(addr),.inst(inst));

    initial begin
        
        $monitor($time,"addr=%h,instdata=%h",addr,inst);
        clk<=1;
        ce<=0;
        #20
        ce<=1;
        for(i=0;i<40;i=i+1)begin
            addr<=i;
            #20;
        end
    end

    always #10 clk<=~clk;

endmodule
```





仿真结果如下图所示：[自己动手写CPU里的某一章文件]

![2021-10-27-inst_rom](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-27-inst_rom.png)

自己手写一份inst_rom.data

如下所示：

```txt
34011100
34020020
3403ff00
3404ffff
```

仿真结果如下图所示：

![2021-10-27-inst_rom2](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-27-inst_rom2.png)



## 取指模块



将pc模块和inst_rom模块连成一个大的电路模块，功能为每个时钟周期从指令存储器中取出一条指令。

RTL代码：

pc模块：

```verilog
`timescale 1ns / 1ps
module pc(
    input wire rst,
    input wire clk,
    output reg[31:0] pc,
    output reg ce
    );

    always @(posedge clk) begin
        if(rst==1)begin
            ce<=0;
        end else begin
            ce<=1;
        end
    end

    always @(posedge clk) begin
        if(ce==0)begin
            pc<=32'h0;
        end else begin
            pc<=pc+32'h4;
        end
    end
    
endmodule
```

inst_rom模块：

```verilog
`timescale 1ns / 1ps
module inst_rom(
    input wire ce,
    input wire [31:0] addr,
    output reg [31:0] inst
    );

    reg [31:0] inst_mem[0:131071];
    initial $readmemh("D:/language/Verilog/test3/test3.srcs/sources_1/new/inst_rom.data",inst_mem);
    always @(*) begin
        if(ce==1'b0)begin
            inst<=32'b0;
        end else begin
            inst<=inst_mem[addr[18:2]];
        end
    end
    
endmodule
```

inst_fetch取指模块：

```verilog
`timescale 1ns / 1ps
module inst_fetch(
    input wire rst,
    input wire clk,
    output wire[31:0] inst_o
    );
    wire [31:0] pc;
    wire ce;
    pc pc0(.clk(clk),.rst(rst),.pc(pc),.ce(ce));
    inst_rom inst_rom0(.ce(ce),.addr(pc),.inst(inst_o));
    
endmodule
```



激励文件：

```verilog
`timescale 1ns / 1ps
module test_tb(

    );
    reg clk;
    reg rst;
    wire [31:0] inst_o;

    inst_fetch inst_fetch0(.rst(rst),.clk(clk),.inst_o(inst_o));

    initial begin
        clk<=1;
        rst<=0;
        #30
        rst<=1;
        #100
        rst<=0;
        #1000 $finish;
    end

    always #10 clk<=~clk;

endmodule
```





仿真结果如下图所示：

![2021-10-27-pc_fetch](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-27-pc_fetch.png)



![2021-10-27-pc_fetch2](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-27-pc_fetch2.png)



## 数据存储器 data_ram

![2021-10-29-data_rom3](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-29-data_rom3.png)

Sel=0001 0010 0100 1000  1111

sel为低两位的地址信息，**即对4个8位存储器的选择信号**，**某一位为1译为为选择该寄存器**



4个8位存储器 $2^{17}-1=131,072-1$



同步写，异步读的功能

**同步写：时序逻辑电路，受时钟信号控制，在时钟上升沿进行赋值，即需要经过两个时钟周期才能观察到波形图的结果；**

**异步读：组合逻辑电路，在一个时钟周期内即可观察到结果。**



+ 字扩展：$8bit*131072$

+ 位扩展：$(8bit*4)*131072$

RTL代码：

```verilog
`timescale 1ns / 1ps
module data_ram(
    input wire clk,
    input wire ce,
    input wire we,
    input wire [31:0] addr,
    input wire [3:0] sel,
    input wire [31:0] data_i,
    output reg [31:0] data_o
    );

    reg [7:0] data_mem0[0:131071];
    reg [7:0] data_mem1[0:131071];
    reg [7:0] data_mem2[0:131071];
    reg [7:0] data_mem3[0:131071];
    
    always @(posedge clk ) begin
        if(ce==1'b0) begin
            data_o<=32'h00000000;
        end else if(we==1'b1) begin
            if(sel[3]==1'b1) begin
                data_mem3[addr[18:2]]<=data_i[31:24];
            end if(sel[2]==1'b1) begin
                data_mem2[addr[18:2]]<=data_i[23:16];
            end if(sel[1]==1'b1) begin
                data_mem1[addr[18:2]]<=data_i[15:8];
            end if(sel[0]==1'b1) begin
                data_mem0[addr[18:2]]<=data_i[7:0];
            end
        end
    end
    
    always @(*) begin
        if(ce==1'b0)begin
            data_o<=32'h00000000;//32'b0
        end else if(we==1'b0) begin
            data_o<={data_mem3[addr[18:2]],
                     data_mem2[addr[18:2]],
                     data_mem1[addr[18:2]],
                     data_mem0[addr[18:2]]};
        end else begin
            data_o<=32'h00000000;
        end
    end
    
endmodule
```



激励文件：

```verilog
`timescale 1ns / 1ps
module test_tb(

    );
    
    reg clk;
    reg ce;
    reg we;
    reg [31:0] addr;
    reg [3:0] sel;
    reg [31:0] data_i;
    wire [31:0] data_o;

    integer i,j,k;

    data_ram data_ram0(.clk(clk),.ce(ce),.we(we),
                       .addr(addr),.sel(sel),
                       .data_i(data_i),.data_o(data_o));

    initial clk=1;
    always #10 clk<=~clk;
    initial begin
        ce=0;
        we=0;
        sel=4'b0001;
        data_i=32'hfedcba98;
        addr=0;
        #100 ce=1;
        we=1;
        for(j=0;j<10;j=j+1)begin
            sel=4'b0001;
            for (i=0;i<4;i=i+1) begin
                #40;
                addr=addr+1;
                sel=sel<<1;
                data_i=data_i-32'h01010101;
            end
        end
        #40;
        we=0;

        for(k=0;k<40;k=k+1)begin
            addr<=k;
            #20;
        end
    end
    
endmodule
```



仿真结果如下图所示：

测试写数据

在芯片使能信号(ce)有效时，写使能信号(we)有效，此时数据存储器进行数据写操作，将测试文件中得到的数据依据字节选择信号(sel)和访问地址addr写入4个8位存储器。

![2021-10-29-data_rom](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/ img/2021-10-29-data_rom.png)

测试读数据

在芯片使能信号(ce)有效时，写使能信号(we)无效，此时数据存储器进行数据读操作，将之前写入4个8位存储器的数据依据访问地址addr读入输出信号data_i中

![2021-10-29-data_rom2](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/ img/2021-10-29-data_rom2.png)



## 运算器 alu

在Verilog学习中常用的编码方式有二进制编码(Binary)、格雷码(Gray-code)编码、独热码(One-hot)编码

逻辑门的实现直接应用了二进制

实际电路中，避免出现计数中可能出现短暂其他代码、导致电路状态错误或输入错误的编码错误【任意两个相邻的代码只有一位二进制数不同】

在通信网络协议栈中，使用八位或者十六位状态的独热码，且系统占用其中一个状态码，余下的可供用户使用【只有一个比特为1，其他全为0】

下面看看十进制数0---7用三种编码如何表示：

| 十进制数 | 二进制 |    独热码    | 格雷码 |
| :------: | :----: | :----------: | :----: |
|    0     | 3’b000 | 8’b0000_0001 | 3’b000 |
|    1     | 3’b001 | 8’b0000_0010 | 3’b001 |
|    2     | 3’b010 | 8’b0000_0100 | 3’b011 |
|    3     | 3’b011 | 8’b0000_1000 | 3’b010 |
|    4     | 3’b100 | 8’b0001_0000 | 3’b110 |
|    5     | 3’b101 | 8’b0010_0000 | 3’b111 |
|    6     | 3’b110 | 8’b0100_0000 | 3’b101 |
|    7     | 3’b111 | 8’b1000_0000 | 3’b100 |

由上面的表格可以看出，独热码的本质就是一位表示一个状态，有几个状态就有几位，格雷码本质上为卡诺图的一条龙遍历。

> 总结三种编码的优缺点：

二进制编码：
    优点：属于压缩状态编码，使用的触发器位数少，可以直接比较大小和算术运算。
    缺点：译码复杂；相邻状态变换时，多位发生改变，电噪声大，转换速度较慢，易出错；

独热码：
    优点：状态比较时仅仅需要比较一个位，从而一定程度上简化了译码逻辑，译码简单，减少了毛刺产生的概率。
    缺点：速度较慢，触发器资源占用较多，面积较大；

格雷码：
    优点：属于压缩状态编码，使用的触发器位数少；相邻状态变换时，仅一位发生改变，电噪声小，转换速度较快；
    缺点：译码复杂，没有固定大小，很难直接进行比较大小和算术运算，需要转换为自然二进制码来判断。

原文链接：https://blog.csdn.net/qq_20222919/article/details/107079043

> 关于运算

逻辑运算：

与&、或|、或非|~、异或^

直接使用逻辑运算符实现



lui高位加载：

直接使用拼接符将低位置0



算术运算：

加法、减法：加法器add【一般为全加器】

乘法：乘法器mul，booth二位乘+华莱士树，状态机

除法：除法器div，状态机



比较运算：

有符号数比较slt：复用加法器，src1-src2，小于置位1，大于等于复位0

| add_src1 | add_src2 | add_result | out  |       解释       |
| :------: | :------: | :--------: | :--: | :--------------: |
|    0     |    1     |  X(0或1)   |  0   |      正-负       |
|    0     |    0     |     1      |  1   |  相减为负，小于  |
|    0     |    0     |     0      |  0   | 相减为正，不小于 |
|    1     |    1     |     1      |  1   |  相减为负，小于  |
|    1     |    1     |     0      |  0   | 相减为正，不小于 |
|    1     |    0     |  X(0或1)   |  1   |      负-正       |

out = (alu_src1&~alu_src2) | (~(alu_src1^alu_src2)&adder_result)

前置条件：alu_src1&~alu_src2满足【负-正】，减法可能存在下溢

​					~(alu_src1^alu_src2)满足【正-负】，减法可能存在上溢



无符号数比较sltu：复用加法器，src1-src2，小于置位1，大于等于复位0

+ 32位无符号数比较，相当于33位有符号数比较【模4补码】
+ {1'b0,src1}-{1'b0,src2}={1'b0,src1}+{1'b1,~src2}+cin
+ 32位加法器：32位加法结果为{adder_cout,adder_result=src1+~src2+cin}
+ 33位加法结果为{adder_cout+1'b1,adder_result}

| adder_cout+1'b1 | out  |
| :-------------: | :--: |
|     0+1'b1      |  1   |
|     1+1'b1      |  0   |

out=~adder_cout



移位运算：

+ 逻辑移位：逻辑左移、逻辑右移

+ 算术移位：算术左移、算术右移

算术左移和逻辑左移一样都是右边补0

逻辑右移很简单，只要将二进制数整体右移，左边补0即可

算术右移符号位要一起移动，并且在左边补上符号位，也就是说，如果符号位是1就补1，符号位是0就补0 



逻辑左移=算术左移

逻辑右移

算术右移

【移位器】：一共有32位，即$2^5$，alu_src1仅需要取前5位

1. 根据移位量低2位即[1:0]位做`第一次移位`
2. 在第一次移位基础上根据移位量[3:2]位做`第二次移位`
3. 在第二次移位基础上根据移位量[4]位做`第三次移位`

> 独热码实现alu_control	



RTL代码：

alu模块：

```verilog
`timescale 1ns / 1ps
module alu(
    input wire [11:0] alu_control,
    input wire [31:0] alu_src1,
    input wire [31:0] alu_src2,
    output wire [31:0] alu_result
    );

    wire alu_add;
    wire alu_sub;
    wire alu_slt;
    wire alu_sltu;
    wire alu_and;
    wire alu_nor;
    wire alu_or;
    wire alu_xor;
    wire alu_sll;
    wire alu_srl;
    wire alu_sra;
    wire alu_lui;

    assign alu_add=alu_control[11];
    assign alu_sub=alu_control[10];
    assign alu_slt=alu_control[9];
    assign alu_sltu=alu_control[8];
    assign alu_and=alu_control[7];
    assign alu_nor=alu_control[6];
    assign alu_or=alu_control[5];
    assign alu_xor=alu_control[4];
    assign alu_sll=alu_control[3];
    assign alu_srl=alu_control[2];
    assign alu_sra=alu_control[1];
    assign alu_lui=alu_control[0]; 

    wire [31:0] add_sub_result;
    wire [31:0] slt_result;
    wire [31:0] sltu_result;
    wire [31:0] and_result;
    wire [31:0] nor_result;
    wire [31:0] or_result;
    wire [31:0] xor_result;
    wire [31:0] sll_result;
    wire [31:0] srl_result;
    wire [31:0] sra_result;
    wire [31:0] lui_result;//高位加载

    //and、nor、or、xor
    assign and_result = alu_src1 & alu_src2;
    assign or_result = alu_src1 | alu_src2;
    assign nor_result = ~or_result;
    assign xor_result = alu_src1 ^ alu_src2;
    //lui
    assign lui_result = {alu_src2[15:0],16'd0};

    wire [31:0] adder_operand1;
    wire [31:0] adder_operand2;
    wire adder_cin;
    wire [31:0] adder_result;
    wire adder_cout;

    //加法器：add、sub、slt、sltu
    assign adder_operand1 = alu_src1;
    assign adder_operand2 = alu_add ? alu_src2 : ~alu_src2;
    assign adder_cin = ~alu_add;//减法需要cin，取反之后的加1
    adder adder_module(
        .operand1(adder_operand1),
        .operand2(adder_operand2),
        .cin(adder_cin),
        .result(adder_result),
        .cout(adder_cout)
    );

    assign add_sub_result = adder_result;

    assign slt_result[31:0] = 31'd0;
    assign slt_result[0] = (alu_src1[31]&~alu_src2[31]) |
                           (~(alu_src1[31]^alu_src2[31])&adder_result[31]);

    assign sltu_result = {31'd0,~adder_cout};

    //移位器：一共有32位，即2的5次方，alu_src1仅需要前5位
    wire [4:0] shf;
    assign shf = alu_src1[4:0];
    wire [1:0] shf_1_0;
    wire [1:0] shf_3_2;
    assign shf_1_0 = shf[1:0];
    assign shf_3_2 = shf[3:2];
    
    //sll
    wire [31:0] sll_step1;
    wire [31:0] sll_step2;
    assign sll_step1 = {32{shf_1_0==2'b00}} & alu_src2
                     | {32{shf_1_0==2'b01}} & {alu_src2[30:0],1'd0}
                     | {32{shf_1_0==2'b10}} & {alu_src2[29:0],2'd0}
                     | {32{shf_1_0==2'b11}} & {alu_src2[28:0],3'd0};
    assign sll_step2 = {32{shf_3_2==2'b00}} & sll_step1
                     | {32{shf_3_2==2'b01}} & {sll_step1[27:0],4'd0}
                     | {32{shf_3_2==2'b10}} & {sll_step1[23:0],8'd0}
                     | {32{shf_3_2==2'b11}} & {sll_step1[19:0],12'd0};
    assign sll_result = shf[4] ? {sll_step2[15:0],16'd0} : sll_step2;
    //srl
    wire [31:0] srl_step1;
    wire [31:0] srl_step2;
    assign srl_step1 = {32{shf_1_0==2'b00}} & alu_src2
                     | {32{shf_1_0==2'b01}} & {1'd0,alu_src2[31:1]}
                     | {32{shf_1_0==2'b10}} & {2'd0,alu_src2[31:2]}
                     | {32{shf_1_0==2'b11}} & {3'd0,alu_src2[31:3]};
    assign srl_step2 = {32{shf_3_2==2'b00}} & srl_step1
                     | {32{shf_3_2==2'b01}} & {4'd0,srl_step1[31:4]}
                     | {32{shf_3_2==2'b10}} & {8'd0,srl_step1[31:8]}
                     | {32{shf_3_2==2'b11}} & {12'd0,srl_step1[31:12]};
    assign srl_result = shf[4] ? {16'd0,srl_step2[31:16]} : srl_step2;
    //sra
    wire [31:0] sra_step1;
    wire [31:0] sra_step2;
    assign sra_step1 = {32{shf_1_0==2'b00}} & alu_src2
                     | {32{shf_1_0==2'b01}} & {alu_src2[31],alu_src2[31:1]}
                     | {32{shf_1_0==2'b10}} & { {2{alu_src2[31]} },alu_src2[31:2]}
                     | {32{shf_1_0==2'b11}} & { {3{alu_src2[31]} },alu_src2[31:3]};
    assign sra_step2 = {32{shf_3_2==2'b00}} & sra_step1
                     | {32{shf_3_2==2'b01}} & { {4{sra_step1[31]} },sra_step1[31:4]}
                     | {32{shf_3_2==2'b10}} & { {8{sra_step1[31]} },sra_step1[31:8]}
                     | {32{shf_3_2==2'b11}} & { {12{sra_step1[31]} },sra_step1[31:12]};
    assign sra_result = shf[4] ? { {16{sra_step2[31]} },sra_step2[31:16]} : sra_step2;
    
    assign alu_result = (alu_add | alu_sub) ? add_sub_result[31:0] :
                         alu_slt ? slt_result :
                         alu_sltu ? sltu_result :
                         alu_and ? and_result :
                         alu_nor ? nor_result :
                         alu_or ? or_result :
                         alu_xor ? xor_result :
                         alu_sll ? sll_result :
                         alu_srl ? srl_result :
                         alu_sra ? sra_result :
                         alu_lui ? lui_result :
                         32'd0;

endmodule
```



adder模块：

```verilog
`timescale 1ns / 1ps

module adder(
    input [31:0] operand1,
    input [31:0] operand2,
    input cin,
    output [31:0] result,
    output cout
    );

    assign {cout,result} = operand1 + operand2 + cin;
    
endmodule
```



激励文件：

```verilog
`timescale 1ns / 1ps
module test_tb(

    );
    
    reg [11:0] alu_control;
    reg [31:0] alu_src1;
    reg [31:0] alu_src2;
    wire [31:0] alu_result;

    integer i;

    alu alu0(.alu_control(alu_control),
             .alu_src1(alu_src1),
             .alu_src2(alu_src2),
             .alu_result(alu_result));

    initial begin
        alu_control=11'b0000_0000_0001;
        alu_src1=32'h1257_89Ab;
        alu_src2=32'hFEAB_BC76;
        #20;
        for(i=0;i<12;i=i+1)begin
            $monitor("alusrc1=%h,alu_control=%b,alusrc2=%h,alu_result",
            alu_src1,alu_control,alu_src2,alu_result);
            #20;
            alu_control=alu_control<<1;
        end
        #40 $finish;
        
    end
endmodule
```



仿真结果如下图所示：

![2021-10-30-alu](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-30-alu.png)



>编码译码方式实现alu_control

使用`编码法`对指令进行编码。

指令编码对应关系：

| ALU 操作             | MIPS相关指令 | 编码法 |
| -------------------- | ------------ | ------ |
| 加法                 | ADD          | 0000   |
| 减法                 | SUB          | 0001   |
| 有符号比较，小于置位 | SLT          | 0010   |
| 无符号比较，小于置位 | SLTU         | 0011   |
| 按位与               | AND          | 0100   |
| 按位或非             | NOR          | 0101   |
| 按位或               | OR           | 0110   |
| 按位异或             | XOR          | 0111   |
| 逻辑左移             | SLL          | 1000   |
| 逻辑右移             | SRL          | 1001   |
| 算术右移             | SRA          | 1011   |
| 高位加载             | LUI          | 1111   |

注：由于本次实验只有12条指令，所以编码位数为4。比赛中要求实现57条指令，所以编码位数为6，包括但不限于除4条非对齐指令外的所有MIPS I指令以及MIPS32中的ERET指令，有14条算术运算指令、8条逻辑运算指令，6条移位指令、8条分支跳转指令、4条数据移动指令、2条自陷指令、12条访存指令、3条特权指令(中断)。



RTL代码：

```verilog
`timescale 1ns / 1ps
`include "defines.v"
module alu(
    input wire rst,
    input wire [3:0] alu_control,
    input wire [31:0] alu_src1,
    input wire [31:0] alu_src2,
    output reg [31:0] alu_result
    );

    wire [31:0] alu_src2_mux;
    wire [31:0] result_sum;
    wire [31:0] src1_lt_src2;
    //add、sub
    assign alu_src2_mux = ((alu_control==`SUB_OP)||(alu_control==`SLT_OP)) ? (~alu_src2)+1 : alu_src2;
    assign result_sum = alu_src1 + alu_src2_mux;
    //slt、sltu
    assign src1_lt_src2 = (alu_control==`SLT_OP) ?
                          ((alu_src1[31]&&!alu_src2[31]) ||
                           (!alu_src1[31]&&!alu_src2[31]&&result_sum[31]) ||
                           (alu_src1[31]&&alu_src2[31]&&result_sum[31]))
                           //有符号比较，直接将三种为1的情况通过逻辑表达式列出来
                          : (alu_src1<alu_src2);
                          //不是，则为无符号比较，直接输出比较结果
    
    always @(*) begin
        if(rst==1'b1)begin
            alu_result=32'h00000000;
        end else begin
            case (alu_control)
                `ADD_OP,`SUB_OP:begin
                    alu_result = result_sum;
                end
                `SLT_OP,`SLTU_OP:begin
                    alu_result = src1_lt_src2;
                end
                //and、nor、or、xor
                `AND_OP:begin
                    alu_result = alu_src1 & alu_src2;
                end
                `NOR_OP:begin
                    alu_result = ~(alu_src1 | alu_src2);
                end
                `OR_OP:begin
                    alu_result = alu_src1 | alu_src2;
                end
                `XOR_OP:begin
                    alu_result = alu_src1 ^ alu_src2;
                end
                //sll、srl、sra
                `SLL_OP:begin
                    alu_result = alu_src2<<alu_src1[4:0];
                end
                `SRL_OP:begin
                    alu_result = alu_src2>>alu_src1[4:0];
                end
                `SRA_OP:begin
                    alu_result = ({32{alu_src2[31]}} << (6'd32-{1'b0,alu_src1[4:0]}))
                    //6'd32-{1'b0,alu_src1[4:0]}表示32-右移位数=当前保留位数
                                 | alu_src2>>alu_src1[4:0];
                    //将alu_src2逻辑右移，并将符号位扩展至alu_src1位(符号位左移32-alu_src1位)
                end
                //lui
                `LUI_OP:begin
                    alu_result = {alu_src2[15:0],16'd0};
                end
                default:begin
                    alu_result = 32'b0;
                end 
            endcase
        end
    end

endmodule
```

defines模块代码：

```verilog
`define ADD_OP 4'b0000
`define SUB_OP 4'b0001
`define SLT_OP 4'b0010
`define SLTU_OP 4'b0011
`define AND_OP 4'b0100
`define NOR_OP 4'b0101
`define OR_OP 4'b0110
`define XOR_OP 4'b0111
`define SLL_OP 4'b1000
`define SRL_OP 4'b1001
`define SRA_OP 4'b1010
`define LUI_OP 4'b1011
```

激励文件：

```verilog
`timescale 1ns / 1ps
module test_tb(

    );
    reg rst;
    reg [3:0] alu_control;
    reg [31:0] alu_src1;
    reg [31:0] alu_src2;
    wire [31:0] alu_result;

    integer i;

    alu alu0(.rst(rst),
             .alu_control(alu_control),
             .alu_src1(alu_src1),
             .alu_src2(alu_src2),
             .alu_result(alu_result));

    initial begin
        rst=1'b1;
        #20;
        alu_control=4'b0000;
        alu_src1=32'h1257_89Ab;
        alu_src2=32'hFEAB_BC76;
        #20;
        rst=1'b0;
        for(i=0;i<12;i=i+1)begin
            $monitor("alusrc1=%h,alu_control=%b,alusrc2=%h,alu_result",
            alu_src1,alu_control,alu_src2,alu_result);
            #20;
            alu_control=alu_control+1;
        end
        #40 $finish;
    end
    
endmodule
```



仿真结果如下图所示：

![2021-10-30-alu2](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-30-alu2.png)



wire —— assign

reg——always



## 译码器 id



译码关于inst的位数取用，参考每条指令的指令格式图

```
ADD $5,$1,$2
000000 00001 00010 00101 00000 100000
0000    00 00    001 0    0010     0010    1 000    00 10    0000
0 0 2 2 2 8 2 0

SUB $6,$4,$3
000000 00100 00011 00110 00000 100010
0000    00 00    100 0    0011     0011    0 000    00 10    0010
0 0 8 3 3 0 2 2

OR $7,$2,$1
000000 00010 00001 00111 00000 100101
0000    00 00    010 0    0001     0011    1 000    00 10    0101
0 0 4 1 3 8 2 5

AND $10,$8,$9
000000 01000 01001 01010 00000 100100
0000    00 01    000 0    1001     0101    0 000    00 10    0100
0 1 0 9 5 0 2 4

XOR $13,$12,$11
000000 01100 01011 01101 00000 100110
0000    00 01    100 0    1011     0110    1 000    00 10    0110
0 1 8 B 6 8 2 6

NOR $14,$15,$16
000000 01111 10000 01110 00000 100111
0000    00 01    111 1    0000     0111    0 000    00 10    0111
0 1 F 0 7 0 2 7

SLT $17,$18,$19
000000 10010 10011 10001 00000 101010
0000    00 10    010 1    0011     1000    1 000    00 10    1010
0 2 5 3 8 8 2 A

SLTU $20,$21,$22
000000 10101 10110 10100 00000 101011
0000    00 10    101 1    0110     1010    0 000    00 10    1011
0 2 B 6 A 0 2 B

SLL $23,$24,6
000000 00000 11000 10111 00110 000000
0000    00 00    000 1    1000     1011    1 001    10 00    0000
0 0 1 8 B 9 8 0

SRL $25,$25,7
000000 00000 11001 11001 00111 000010
0000    00 00    000 1    1001     1100    1 001    11 00    0010
0 0 1 9 C 9 C 2

SRA $26,$26,8
000000 00000 11010 11010 01000 000011
0000    00 00    000 1    1010     1101    0 010    00 00    0011
0 0 1 A D 2 0 3

LUI $30,0x1100
001111 00000 11110 0001000100000000
0011    11 00    000 1    1110    0001    0001    0000    0000
3 C 1 E 1 1 0 0
```

data

```
0 0 2 2 2 8 2 0
0 0 8 3 3 0 2 2
0 0 4 1 3 8 2 5
0 1 0 9 5 0 2 4
0 1 8 B 6 8 2 6
0 1 F 0 7 0 2 7
0 2 5 3 8 8 2 A
0 2 B 6 A 0 2 B
0 0 1 8 B 9 8 0
0 0 1 9 C 9 C 2
0 0 1 A D 2 0 3
3 C 1 E 1 1 0 0
```

```
00222820
00833022
00413825
01095024
018B6826
01F07027
0253882A
02B6A02B
0018B980
0019C9C2
001AD203
3C1E1100
```



RTL代码：

```verilog
`timescale 1ns / 1ps
`include "defines.v"
module id(
    input wire rst,
    input wire [31:0] inst_i,
    input wire [31:0] reg1_data_i,
    input wire [31:0] reg2_data_i,

    //32个通用寄存器regfile
    output reg reg1_read_o,
    output reg reg2_read_o,
    output reg [4:0] reg1_addr_o,
    output reg [4:0] reg2_addr_o,

    output reg [3:0] aluop_o,
    output reg [31:0] reg1_o,
    output reg [31:0] reg2_o,
    output reg [4:0] wd_o,//目的寄存器地址
    output reg wreg_o//是否要写入目的寄存器
    );

    //将指令分为4块分别译码
    wire [5:0] op = inst_i[31:26];
    wire [4:0] op2 = inst_i[10:6];
    wire [5:0] op3 = inst_i[5:0];
    wire [4:0] po4 = inst_i[20:16];
    reg [31:0] imm;
    reg instvalid;//指示指令是否有效，即是否在CPU中实现了该指令
    //若指令信号无效，则进入中断处理

    always @(*) begin
        if(rst==1'b1)begin
            aluop_o<=`NOP_OP;//空指令
            wd_o<=5'b00000;
            wreg_o<=1'b0;
            instvalid<=1'b0;
            reg1_read_o<=1'b0;
            reg2_read_o<=1'b0;
            reg1_addr_o<=5'b00000;
            reg2_addr_o<=5'b00000;
            imm<=32'h0;
        end else begin
            aluop_o<=`NOP_OP;
            wd_o<=inst_i[15:11];//目的寄存器地址
            wreg_o<=1'b0;
            instvalid<=1'b1;//[指令无效]
            reg1_read_o<=1'b0;
            reg2_read_o<=1'b0;
            reg1_addr_o<=inst_i[25:21];
            reg2_addr_o<=inst_i[20:16];
            imm<=32'h00000000;
            case (op)
                `EXE_SPECIAL_INST:begin
                    case (op2)
                        5'b00000:begin
                            case (op3)
                                `EXE_OR:begin
                                    aluop_o<=`OR_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=1'b0;//[指令有效]
                                end
                                `EXE_AND:begin
                                    aluop_o<=`AND_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=1'b0;//[指令有效]
                                end
                                `EXE_XOR:begin
                                    aluop_o<=`XOR_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=1'b0;//[指令有效]
                                end
                                `EXE_NOR:begin
                                    aluop_o<=`NOR_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=1'b0;//[指令有效]
                                end
                                `EXE_SLT:begin
                                    aluop_o<=`SLT_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=1'b0;
                                end
                                `EXE_SLTU:begin
                                    aluop_o<=`SLTU_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=1'b0;
                                end
                                `EXE_ADD:begin
                                    aluop_o<=`ADD_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=1'b0;
                                end
                                `EXE_SUB:begin
                                    aluop_o<=`SUB_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=1'b0;
                                end
                                default:begin
                                    //op3 over
                                end
                            endcase
                        end
                        default:begin
                            //op2 over
                        end
                    endcase
                end
                `EXE_LUI:begin
                    aluop_o<=`LUI_OP;
                    wreg_o<=1'b1;
                    reg1_read_o<=1'b1;
                    reg2_read_o<=1'b0;
                    instvalid<=1'b0;
                    imm<={inst_i[15:0],16'h0};
                    wd_o<=inst_i[20:16];
                end
                default:begin
                    //op over
                end
            endcase
            if(inst_i[31:21]==11'b00000000000)begin
                //sll、srl、sra 31位-21位均为0
                if(op3==`EXE_SLL)begin
                    aluop_o<=`SLL_OP;
                    wreg_o<=1'b1;
                    reg1_read_o<=1'b0;
                    reg2_read_o<=1'b1;
                    instvalid<=1'b0;
                    imm[4:0]<=inst_i[10:6];
                    wd_o<=inst_i[15:11];
                end else if(op3==`EXE_SRL)begin
                    aluop_o<=`SRL_OP;
                    wreg_o<=1'b1;
                    reg1_read_o<=1'b0;
                    reg2_read_o<=1'b1;
                    instvalid<=1'b0;
                    imm[4:0]<=inst_i[10:6];
                    wd_o<=inst_i[15:11];
                end else if(op3==`EXE_SRA)begin
                    aluop_o<=`SRA_OP;
                    wreg_o<=1'b1;
                    reg1_read_o<=1'b0;
                    reg2_read_o<=1'b1;
                    instvalid<=1'b0;
                    imm[4:0]<=inst_i[10:6];
                    wd_o<=inst_i[15:11];
                end
            end
        end
    end
    
    always @(*) begin
        if(rst==1'b1)begin
            reg1_o<=32'h00000000;
        end else if(reg1_read_o==1'b1)begin
            reg1_o<=reg1_data_i;
        end else if(reg1_read_o==1'b0)begin
            reg1_o<=imm;
        end else begin
            reg1_o<=32'h00000000;
        end
    end

    always @(*) begin
        if(rst==1'b1)begin
            reg2_o<=32'h00000000;
        end else if(reg2_read_o==1'b1)begin
            reg2_o<=reg2_data_i;
        end else if(reg2_read_o==1'b0)begin
            reg2_o<=imm;
        end else begin
            reg2_o<=32'h00000000;
        end
    end

endmodule
```

defines模块代码：

```verilog
`define ADD_OP 4'b0000
`define SUB_OP 4'b0001
`define SLT_OP 4'b0010
`define SLTU_OP 4'b0011
`define AND_OP 4'b0100
`define NOR_OP 4'b0101
`define OR_OP 4'b0110
`define XOR_OP 4'b0111
`define SLL_OP 4'b1000
`define SRL_OP 4'b1001
`define SRA_OP 4'b1010
`define LUI_OP 4'b1011
`define NOP_OP 4'b1111

`define EXE_AND  6'b100100
`define EXE_OR   6'b100101
`define EXE_XOR 6'b100110
`define EXE_NOR 6'b100111
`define EXE_LUI 6'b001111

`define EXE_SLL  6'b000000
`define EXE_SRL  6'b000010
`define EXE_SRA  6'b000011

`define EXE_SLT  6'b101010
`define EXE_SLTU  6'b101011

`define EXE_ADD  6'b100000
`define EXE_SUB  6'b100010

`define EXE_SPECIAL_INST 6'b000000
```



激励文件：

```verilog
`timescale 1ns / 1ps
module test_tb(

    );
    
    reg rst;
    reg [31:0] inst_i;
    reg [31:0] reg1_data_i;
    reg [31:0] reg2_data_i;
    wire reg1_read_o;
    wire reg2_read_o;
    wire [4:0] reg1_addr_o;
    wire [4:0] reg2_addr_o;
    wire [3:0] aluop_o;
    wire [31:0] reg1_o;
    wire [31:0] reg2_o;
    wire [4:0] wd_o;
    wire wreg_o;

    reg [31:0] inst_array[0:11];
    integer i;

    initial begin
        $readmemh("D:/language/Verilog/test2/test2.srcs/sources_1/new/inst_rom.data",inst_array);
    end

    id id0(.rst(rst),.inst_i(inst_i),
           .reg1_data_i(reg1_data_i),.reg2_data_i(reg2_data_i),
           .reg1_read_o(reg1_read_o),.reg2_read_o(reg2_read_o),
           .reg1_addr_o(reg1_addr_o),.reg2_addr_o(reg2_addr_o),
           .aluop_o(aluop_o),.reg1_o(reg1_o),.reg2_o(reg2_o),
           .wd_o(wd_o),.wreg_o(wreg_o));

    initial begin
        rst=1'b1;
        #100
        rst=1'b0;
        reg1_data_i=32'h12345678;
        reg2_data_i=32'hfedcba98;
        for (i=0;i<12;i=i+1) begin
            inst_i=inst_array[i];
            #20;
        end
        #20 $stop;
    end
    
endmodule
```





仿真结果如下图所示：



## 单周期CPU





RTL代码：

pc_reg模块代码：

取指阶段，从pc取到地址，并将下一条指令加4

```verilog
`timescale 1ns / 1ps
module pc_reg(
    input wire rst,
    input wire clk,
    output reg[31:0] pc,
    output reg ce
    );

    always @(posedge clk) begin
        if(rst==1)begin
            ce<=0;
        end else begin
            ce<=1;
        end
    end

    always @(posedge clk) begin
        if(ce==0)begin
            pc<=32'h0;
        end else begin
            pc<=pc+32'h4;
        end
    end
endmodule
```

id模块代码：

译码阶段，将取到的指令进行解释，并将运算所需的各种信号传递到下一阶段

```verilog
`timescale 1ns / 1ps
`include "defines.v"
module id(
    input wire rst,
    input wire [31:0] inst_i,
    input wire [31:0] reg1_data_i,
    input wire [31:0] reg2_data_i,

    //32个通用寄存器regfile
    output reg reg1_read_o,
    output reg reg2_read_o,
    output reg [4:0] reg1_addr_o,
    output reg [4:0] reg2_addr_o,

    output reg [3:0] aluop_o,
    output reg [31:0] reg1_o,
    output reg [31:0] reg2_o,
    output reg [4:0] wd_o,//目的寄存器地址
    output reg wreg_o//是否要写入目的寄存器
    );

    //将指令分为4块分别译码
    wire [5:0] op = inst_i[31:26];
    wire [4:0] op2 = inst_i[10:6];
    wire [5:0] op3 = inst_i[5:0];
    wire [4:0] po4 = inst_i[20:16];
    reg [31:0] imm;
    reg instvalid;//指示指令是否有效，即是否在CPU中实现了该指令
    //若指令信号无效，则进入中断处理

    always @(*) begin
        if(rst==1'b1)begin
            aluop_o<=`NOP_OP;//空指令
            wd_o<=5'b00000;
            wreg_o<=1'b0;
            instvalid<=1'b0;
            reg1_read_o<=1'b0;
            reg2_read_o<=1'b0;
            reg1_addr_o<=5'b00000;
            reg2_addr_o<=5'b00000;
            imm<=32'h0;
        end else begin
            aluop_o<=`NOP_OP;
            wd_o<=inst_i[15:11];//目的寄存器地址
            wreg_o<=1'b0;
            instvalid<=1'b1;//[指令无效]
            reg1_read_o<=1'b0;
            reg2_read_o<=1'b0;
            reg1_addr_o<=inst_i[25:21];
            reg2_addr_o<=inst_i[20:16];
            imm<=32'h00000000;
            case (op)
                `EXE_SPECIAL_INST:begin
                    case (op2)
                        5'b00000:begin
                            case (op3)
                                `EXE_OR:begin
                                    aluop_o<=`OR_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=1'b0;//[指令有效]
                                end
                                `EXE_AND:begin
                                    aluop_o<=`AND_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=1'b0;//[指令有效]
                                end
                                `EXE_XOR:begin
                                    aluop_o<=`XOR_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=1'b0;//[指令有效]
                                end
                                `EXE_NOR:begin
                                    aluop_o<=`NOR_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=1'b0;//[指令有效]
                                end
                                `EXE_SLT:begin
                                    aluop_o<=`SLT_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=1'b0;
                                end
                                `EXE_SLTU:begin
                                    aluop_o<=`SLTU_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=1'b0;
                                end
                                `EXE_ADD:begin
                                    aluop_o<=`ADD_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=1'b0;
                                end
                                `EXE_SUB:begin
                                    aluop_o<=`SUB_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=1'b0;
                                end
                                default:begin
                                    //op3 over
                                end
                            endcase
                        end
                        default:begin
                            //op2 over
                        end
                    endcase
                end
                `EXE_LUI:begin
                    aluop_o<=`LUI_OP;
                    wreg_o<=1'b1;
                    reg1_read_o<=1'b1;
                    reg2_read_o<=1'b0;
                    instvalid<=1'b0;

                    imm<={inst_i[15:0],16'h0};
                    wd_o<=inst_i[20:16];
                end
                default:begin
                    //op over
                end
            endcase
            if(inst_i[31:21]==11'b00000000000)begin
                //sll、srl、sra 31位-21位均为0
                if(op3==`EXE_SLL)begin
                    aluop_o<=`SLL_OP;
                    wreg_o<=1'b1;
                    reg1_read_o<=1'b0;
                    reg2_read_o<=1'b1;
                    instvalid<=1'b0;

                    imm[4:0]<=inst_i[10:6];
                    wd_o<=inst_i[15:11];
                end else if(op3==`EXE_SRL)begin
                    aluop_o<=`SRL_OP;
                    wreg_o<=1'b1;
                    reg1_read_o<=1'b0;
                    reg2_read_o<=1'b1;
                    instvalid<=1'b0;

                    imm[4:0]<=inst_i[10:6];
                    wd_o<=inst_i[15:11];
                end else if(op3==`EXE_SRA)begin
                    aluop_o<=`SRA_OP;
                    wreg_o<=1'b1;
                    reg1_read_o<=1'b0;
                    reg2_read_o<=1'b1;
                    instvalid<=1'b0;

                    imm[4:0]<=inst_i[10:6];
                    wd_o<=inst_i[15:11];
                end
            end
        end
    end
    
    always @(*) begin
        if(rst==1'b1)begin
            reg1_o<=32'h00000000;
        end else if(reg1_read_o==1'b1)begin
            reg1_o<=reg1_data_i;
        end else if(reg1_read_o==1'b0)begin
            reg1_o<=imm;
        end else begin
            reg1_o<=32'h00000000;
        end
    end

    always @(*) begin
        if(rst==1'b1)begin
            reg2_o<=32'h00000000;
        end else if(reg2_read_o==1'b1)begin
            reg2_o<=reg2_data_i;
        end else if(reg2_read_o==1'b0)begin
            reg2_o<=imm;
        end else begin
            reg2_o<=32'h00000000;
        end
    end

endmodule
```

alu模块代码：

执行阶段，通过上一级传递过来的信号信息，进行相关指令的判断和运算

新增内容：目的寄存器地址(wd)和是否要写入目的寄存器(wreg)两种信号的传递，包括但不限于在always语句中添加语句`wd_o=wd_i;`和` wreg_o=wreg_i;`

```verilog
`timescale 1ns / 1ps
`include "defines.v"
module alu(
    input wire rst,
    input wire [3:0] alu_control,
    input wire [31:0] alu_src1,
    input wire [31:0] alu_src2,
    input wire [4:0] wd_i,//new:目的寄存器地址[信号传递]
    input wire wreg_i,//new:是否要写入目的寄存器[信号传递]
    output reg [31:0] alu_result,
    output reg [4:0] wd_o,//new:目的寄存器地址[信号传递]
    output reg wreg_o//new:是否要写入目的寄存器[信号传递]
    );

    wire [31:0] alu_src2_mux;
    wire [31:0] result_sum;
    wire [31:0] src1_lt_src2;
    //add、sub
    assign alu_src2_mux = ((alu_control==`SUB_OP)||(alu_control==`SLT_OP)) ? (~alu_src2)+1 : alu_src2;
    assign result_sum = alu_src1 + alu_src2_mux;
    //slt、sltu
    assign src1_lt_src2 = (alu_control==`SLT_OP) ?
                          ((alu_src1[31]&&!alu_src2[31]) ||
                           (!alu_src1[31]&&!alu_src2[31]&&result_sum[31]) ||
                           (alu_src1[31]&&alu_src2[31]&&result_sum[31]))
                           //有符号比较，直接将三种为1的情况通过逻辑表达式列出来
                          : (alu_src1<alu_src2);
                          //不是，则为无符号比较，直接输出比较结果
    
    always @(*) begin
        if(rst==1'b1)begin
            alu_result=32'h00000000;
            wd_o=5'b00000;
            wreg_o=1'b0;
        end else begin
            wd_o=wd_i;//new:目的寄存器地址[信号传递]
            wreg_o=wreg_i;//new:是否要写入目的寄存器[信号传递]
            case (alu_control)
                `ADD_OP,`SUB_OP:begin
                    alu_result = result_sum;
                end
                `SLT_OP,`SLTU_OP:begin
                    alu_result = src1_lt_src2;
                end
                //and、nor、or、xor
                `AND_OP:begin
                    alu_result = alu_src1 & alu_src2;
                end
                `NOR_OP:begin
                    alu_result = ~(alu_src1 | alu_src2);
                end
                `OR_OP:begin
                    alu_result = alu_src1 | alu_src2;
                end
                `XOR_OP:begin
                    alu_result = alu_src1 ^ alu_src2;
                end
                //sll、srl、sra
                `SLL_OP:begin
                    alu_result = alu_src2<<alu_src1[4:0];
                end
                `SRL_OP:begin
                    alu_result = alu_src2>>alu_src1[4:0];
                end
                `SRA_OP:begin
                    alu_result = ({32{alu_src2[31]}} << (6'd32-{1'b0,alu_src1[4:0]}))
                    //6'd32-{1'b0,alu_src1[4:0]}表示32-右移位数=当前保留位数
                                | alu_src2>>alu_src1[4:0];
                    //将alu_src2逻辑右移，并将符号位扩展至alu_src1位(符号位左移32-alu_src1位)
                end
                //lui
                `LUI_OP:begin
                    alu_result = alu_src2;
                end
                default:begin
                    alu_result = 32'b0;
                end 
            endcase
        end
    end

endmodule
```

regfile模块代码：

寄存器堆模块，存放数据

新增内容：初始化regs[1]和regs[2]为32'h12345678和32'hfedcba98

```verilog
`timescale 1ns / 1ps
module regfile(
    input wire rst,
    input wire clk,
    input wire [4:0] waddr,
    input wire [31:0] wdata,
    input wire we,
    input wire [4:0] raddr1,
    input wire re1,
    output reg [31:0] rdata1,
    input wire [4:0] raddr2,
    input wire re2,
    output reg [31:0] rdata2
    );

    reg [31:0] regs[0:31];

    integer i;
    initial begin//new:新增初始化内容
        regs[1]=32'h12345678;
        for(i=2;i<32;i=i+1)begin
            regs[i]=regs[i-1]+32'h01010101;
        end
    end


    always @(posedge clk) begin
        if(rst==1'b0)begin
            if((we==1'b1)&&(waddr!=5'b0))begin
                regs[waddr]<=wdata;
            end
        end
    end

    always @(*) begin
        if(rst==1)begin
            rdata1<=32'h0;
        end else if((raddr1==5'b0)&&(re1==1'b1))begin
            rdata1<=32'h0;
        end else if((raddr1==waddr)&&(we==1'b1)&&(re1==1'b1))begin
            rdata1<=wdata;
        end else if(re1==1'b1)begin
            rdata1<=regs[raddr1];
        end else begin
            rdata1<=32'h0;
        end
    end

    always @(*) begin
        if(rst==1)begin
            rdata2<=32'h0;
            //当复位信号（rst==1）有效时，读数据（rdata1和rdata2）为0
        end else if((raddr2==5'b0)&&(re2==1'b1))begin
            rdata2<=32'h0;
            //当复位信号（rst==0）无效时,当读地址为0，读数据为0
        end else if((raddr2==waddr)&&(we==1'b1)&&(re2==1'b1))begin
            rdata2<=wdata;
            //当读写地址相等，且读写使能都有效的时候，读数据为写数据【数据相关：读后写】
        end else if(re2==1'b1)begin
            rdata2<=regs[raddr2];
            //当读使能有效时，读数据为寄存器堆中存储数据
        end else begin
            rdata2<=32'h0;
        end
    end
    
endmodule
```

inst_rom模块代码：

```verilog
`timescale 1ns / 1ps
module inst_rom(
    input wire ce,
    input wire [31:0] addr,
    output reg [31:0] inst
    );

    reg [31:0] inst_mem[0:131071];
    initial $readmemh("D:/language/Verilog/single_cycle/single_cycle.srcs/sources_1/new/inst_rom.data",inst_mem);
    always @(*) begin
        if(ce==1'b0)begin
            inst<=32'b0;
        end else begin
            inst<=inst_mem[addr[18:2]];
        end
    end
    
endmodule
```

defines模块代码：

```verilog
`define ADD_OP 4'b0000
`define SUB_OP 4'b0001
`define SLT_OP 4'b0010
`define SLTU_OP 4'b0011
`define AND_OP 4'b0100
`define NOR_OP 4'b0101
`define OR_OP 4'b0110
`define XOR_OP 4'b0111
`define SLL_OP 4'b1000
`define SRL_OP 4'b1001
`define SRA_OP 4'b1010
`define LUI_OP 4'b1011
`define NOP_OP 4'b1111

`define EXE_AND  6'b100100
`define EXE_OR   6'b100101
`define EXE_XOR 6'b100110
`define EXE_NOR 6'b100111
`define EXE_LUI 6'b001111

`define EXE_SLL  6'b000000
`define EXE_SRL  6'b000010
`define EXE_SRA  6'b000011

`define EXE_SLT  6'b101010
`define EXE_SLTU  6'b101011

`define EXE_ADD  6'b100000
`define EXE_SUB  6'b100010
`define EXE_SPECIAL_INST 6'b000000
```

***

下列是对之前完成模块的封装。

single_cycle_cpu模块：

将pc模块、id模块、alu模块、regfile模块例化，并进行信号的连接

```verilog
`timescale 1ns / 1ps

module single_cycle_cpu(
    input wire clk,
    input wire rst,
    input wire [31:0] rom_data_i,
    output wire [31:0] rom_addr_o,
    output wire rom_ce_o
    );

    pc_reg pc_reg0(.rst(rst),
                   .clk(clk),
                   .pc(rom_addr_o),
                   .ce(rom_ce_o));

    wire [3:0] id_aluop_o;
    wire [31:0] id_reg1_o;
    wire [31:0] id_reg2_o;
    wire [4:0] id_wd_o;
    wire id_wreg_o;

    wire reg1_read;
    wire reg2_read;
    wire [4:0] reg1_addr;
    wire [4:0] reg2_addr;
    wire [31:0] reg1_data;
    wire [31:0] reg2_data;

    id id0(.rst(rst),.inst_i(rom_data_i),
           .reg1_data_i(reg1_data),.reg2_data_i(reg2_data),
           .reg1_read_o(reg1_read),.reg2_read_o(reg2_read),
           .reg1_addr_o(reg1_addr),.reg2_addr_o(reg2_addr),
           .aluop_o(id_aluop_o),
           .reg1_o(id_reg1_o),.reg2_o(id_reg2_o),
           .wd_o(id_wd_o),.wreg_o(id_wreg_o)
    );

    wire [31:0] wdata_o;
    wire [4:0] wd_o;
    wire wreg_o;

    alu alu0(
        .rst(rst),
        .alu_control(id_aluop_o),
        .alu_src1(id_reg1_o),.alu_src2(id_reg2_o),
        .wd_i(id_wd_o),.wreg_i(id_wreg_o),
        .alu_result(wdata_o),
        .wd_o(wd_o),.wreg_o(wreg_o)
    );

    regfile regfile0(
        .rst(rst),.clk(clk),
        .waddr(wd_o),.wdata(wdata_o),.we(wreg_o),
        .raddr1(reg1_addr),.re1(reg1_read),
        .rdata1(reg1_data),
        .raddr2(reg2_addr),.re2(reg2_read),
        .rdata2(reg2_data)
    );
    
endmodule
```

mips_sopc模块：

将single_cycle_cpu模块、inst_rom模块例化，并进行信号的连接

```verilog
`timescale 1ns / 1ps
module mips_sopc(
    input wire clk,
    input wire rst
    );

    wire [31:0] inst_addr;
    wire [31:0] inst;
    wire rom_ce;

    single_cycle_cpu single_cycle_cpu0(
        .clk(clk),.rst(rst),
        .rom_data_i(inst),
        .rom_addr_o(inst_addr),
        .rom_ce_o(rom_ce)
    );

    inst_rom inst_rom0(
        .ce(rom_ce),
        .addr(inst_addr),
        .inst(inst)
    );
    
endmodule
```

mips_sopc_tb模块：

传递时钟信号(clk)和复位信号(rst)，例化mips_sopc，是mips最顶层的文件

```verilog
`timescale 1ns / 1ps
module mips_sopc_tb(
    );

    reg clk;
    reg rst;
    initial begin
        clk=1'b0;
        forever #10 clk=~clk;
    end
    initial begin
        rst=1;
        #100 rst=0;
        #1000 $stop;
    end
    mips_sopc mips_sopc0(
        .clk(clk),
        .rst(rst)
    );
    
endmodule
```



激励文件：

这里的激励文件为通过inst_rom模块将data文件中的数据进行读入和mips_sopc_tb模块顶层输入的时钟信号(clk)和复位信号(rst)



仿真结果如下图所示：







## 多周期CPU：五级流水CPU

> 引入流水线的概念

参考：

[菜鸟教程——流水线](https://www.runoob.com/w3cnote/verilog-pipeline-design.html)

[verilog实现简单的三级加法流水线](https://blog.csdn.net/wangbowj123/article/details/107213477)

[Verilog十大基本功1（流水线设计Pipeline Design）](https://blog.csdn.net/moshanghuakaizfq/article/details/77717709?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0.no_search_link&spm=1001.2101.3001.4242.1)



单周期

多周期

流水线

超标量流水线



[计算机体系结构——流水线技术](https://wenku.baidu.com/view/adb512b2dd88d0d233d46a81.html)

> 流水线的分类

按照功能的多少来分：

+ 单功能流水线：只能完成一种固定功能的流水线
+ 多功能流水线：流水线的各段可以进行不同的连接，从而实现不同的功能

按同一时间内各段之间的连接方式来分：

+ 静态流水线：在同一时刻，流水线的各段只能按同一种功能的连接方式工作
+ 动态流水线：在同一时刻，流水线的各段可以按不同功能的连接方式工作

按照流水线的级别来分：

+ 部件级流水线、运算操作流水线：【浮点数】求阶差、对阶、尾数相加、规格化
+ 处理机级流水线、指令流水线：取指、译码、执行、访存、写回
+ 处理机间流水线、宏流水线：由两个以上的处理机串行地对同一数据流进行处理，每个处理机完成一项任务

按照数据表示来分：

+ 标量处理机：不具有向量指令和向量数据表示，仅对标量进行流水处理的处理机 MIPS
+ 向量处理机：具有向量指令和向量数据表示的处理机 MFLOPS

按照是否有反馈回路来分：

+ 线性流水线：流水线中的各段串行连接，没有反馈回路
+ 非线性流水线：串行连接+反馈回路

按照流动是否可以乱序来分：

+ 顺序流动流水线：流水线输出端任务流出的顺序与输入端任务流入的顺序相同
+ 异步流动流水线（乱序流水线）：流水线输出端任务流出的顺序与输入端任务流入的顺序不同

> 指令执行的并行性

流水线的基本思想是：把一个重复的过程分解为若干个子过程，每个子过程由专门的功能部件来实现。将多个处理过程在时间上错开，依次通过各功能段，这样每个子过程就可以与其他子过程并行进行。

本质：

+ 数据的处理路径也可以看作是一条生产线，路径上的每个数字处理单元都可以看作是一个阶段，会产生延时。流水线设计就是将路径系统的分割成一个个数字处理单元（阶段），并在各个处理单元之间插入寄存器来暂存中间阶段的数据。被分割的单元能够按阶段并行的执行，相互间没有影响。所以最后流水线设计能够提高数据的吞吐率，即提高数据的处理速度。
+ 将组合逻辑系统地分割，并在各个部分（分级）之间插入寄存器，并暂存中间数据的方法。

优点： 流水线缩短了在一个时钟周期内给的那个信号必须通过的通路长度，增加了数据吞吐量，从而可以提高时钟频率，但也导致了数据的延时。

缺点：各个处理阶段都需要增加寄存器保存中间计算状态，而且多条指令并行执行，势必会导致功耗增加。



简单说，就是增加寄存器保存中间结果，减少寄存器和寄存器之间的组合逻辑

在长延时的逻辑功能块中插入触发器，使复杂的逻辑操作分步完成，减小每个部分的延时

> 二级加法流水线

第一级低 4bit，第二级高 4bit，所以第一个输出需要 2 个时钟周期有效，后面的数据都是 1 个周期

非流水线：

```verilog
module add(
    input  [7:0] a,
    input  [7:0] b,
  	output [8:0] c
);
    assign c[8:0] = {1'd0, a} + {1'd0, b};
    
endmodule
```

改进为二级加法流水线：

```verilog
module adder(
        input          clk,
        input          cin,
        input  [7:0]   cina,
        input  [7:0]   cinb,
  
        output [7:0]   sum,
        output          cout
);
  
  reg            cout;
  reg            cout1; //插入的寄存器
  reg   [3 :0 ]  sum1 ; //插入的寄存器
  reg   [7 :0 ]  sum;
  reg   [3:0]    cina_reg;
  reg   [3:0]    cinb_reg;//插入的寄存器
  
  
  always @(posedge clk) //第一级流水：低 4bit相加
  begin
    {cout1 , sum1} <= cina[3:0] + cinb [3:0] + cin ;
  end
  
  always @(posedge clk)
  begin
    cina_reg <= cina[7:4];
    cinb_reg <= cinb[7:4];
  end
  
  always @(posedge clk) //第二级流水：高 4bit相加
  begin
    {cout ,sum[7:0]} <= { {1'b0,cina_reg[3:0]} + {1'b0,cinb_reg[3:0]} + cout1 ,sum1[3:0]} ;
  end
    
endmodule
```

这里讲到的流水线，主要是一种硬件设计的算法，如上面表述中的流水线设计就是将组合逻辑系统地分割，并在各个部分（分级）之间插入寄存器，并暂存中间数据的方法。

> 三级加法流水线



RTL代码：

```verilog
```



激励文件：

```verilog
```

仿真结果如下图所示：





***

>五级流水CPU

简单的五级数据通路：

1. 取指令周期IF

2. 指令译码/读寄存器周期ID：指令译码和读寄存器是并行进行的。【原因：在该指令格式中，操作码在固定位置——固定字段译码】

3. 执行/有效地址计算周期EX：有效地址计算和执行合并为一个时钟周期，是由于指令集结构中，没有任何指令需要同时计算数据的存储器地址、计算分支指令的目标地址和进行数据处理

   存储器访问

   寄存器-寄存器ALU操作

   寄存器-立即值ALU操作

   分支操作

4. 存储器访问/分支完成周期MEM：Load、Store、分支指令

   存储器访问

   分支操作

5. 写回周期

   寄存器=寄存器型ALU指令

   寄存器-立即值型ALU指令

   Load指令

流水线技术应该解决下列几个问题：

+ 保证不会在同一时钟周期内在同一数据通路资源上做不同的操作

  + 指令存储器i-cache和数据存储器d-cache分开，避免访存冲突
  + 解决对同一寄存器的访问冲突：写后读RAW、读后写WAR、写后写WRW

+ 流水线各段之间需设置流水线寄存器（也称为锁存器）

  把数据和控制信息从一个流水段传送到下一个流水段



将single_cycle_cpu模块替换为pipeline_cpu模块，即增加if_id模块，id_ex模块，ex_mem模块，mem_wb模块

RTL代码：

pc_reg模块代码：

```verilog
`timescale 1ns / 1ps
module pc_reg(
    input wire rst,
    input wire clk,
    output reg[31:0] pc,
    output reg ce
    );

    always @(posedge clk) begin
        if(rst==1)begin
            ce<=0;
        end else begin
            ce<=1;
        end
    end

    always @(posedge clk) begin
        if(ce==0)begin
            pc<=32'h0;
        end else begin
            pc<=pc+32'h4;
        end
    end
    
endmodule
```

if_id模块代码：

```verilog
`timescale 1ns / 1ps
module if_id(
    input wire rst,
    input wire clk,
    input wire [31:0] if_inst,
    output reg [31:0] id_inst
    );

    always @(posedge clk ) begin
        if(rst==1'b1)begin
            id_inst<=32'h00000000;
        end else begin
            id_inst<=if_inst;
        end
    end
    
endmodule
```

id模块代码：

```verilog
`timescale 1ns / 1ps
`include "defines.v"
module id(
    input wire rst,
    input wire [31:0] inst_i,
    input wire [31:0] reg1_data_i,
    input wire [31:0] reg2_data_i,

    //32个通用寄存器regfile
    output reg reg1_read_o,
    output reg reg2_read_o,
    output reg [4:0] reg1_addr_o,
    output reg [4:0] reg2_addr_o,

    output reg [3:0] aluop_o,
    output reg [31:0] reg1_o,
    output reg [31:0] reg2_o,
    output reg [4:0] wd_o,//目的寄存器地址
    output reg wreg_o//是否要写入目的寄存器
    );

    //将指令分为4块分别译码
    wire [5:0] op = inst_i[31:26];
    wire [4:0] op2 = inst_i[10:6];
    wire [5:0] op3 = inst_i[5:0];
    wire [4:0] po4 = inst_i[20:16];
    reg [31:0] imm;
    reg instvalid;//指示指令是否有效，即是否在CPU中实现了该指令
    //若指令信号无效，则进入中断处理

    always @(*) begin
        if(rst==1'b1)begin
            aluop_o<=`NOP_OP;//空指令
            wd_o<=5'b00000;
            wreg_o<=1'b0;
            instvalid<=1'b0;
            reg1_read_o<=1'b0;
            reg2_read_o<=1'b0;
            reg1_addr_o<=5'b00000;
            reg2_addr_o<=5'b00000;
            imm<=32'h0;
        end else begin
            aluop_o<=`NOP_OP;
            wd_o<=inst_i[15:11];//目的寄存器地址
            wreg_o<=1'b0;
            instvalid<=1'b1;//[指令无效]
            reg1_read_o<=1'b0;
            reg2_read_o<=1'b0;
            reg1_addr_o<=inst_i[25:21];
            reg2_addr_o<=inst_i[20:16];
            imm<=32'h00000000;
            case (op)
                `EXE_SPECIAL_INST:begin
                    case (op2)
                        5'b00000:begin
                            case (op3)
                                `EXE_OR:begin
                                    aluop_o<=`OR_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=1'b0;//[指令有效]
                                end
                                `EXE_AND:begin
                                    aluop_o<=`AND_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=1'b0;//[指令有效]
                                end
                                `EXE_XOR:begin
                                    aluop_o<=`XOR_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=1'b0;//[指令有效]
                                end
                                `EXE_NOR:begin
                                    aluop_o<=`NOR_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=1'b0;//[指令有效]
                                end
                                `EXE_SLT:begin
                                    aluop_o<=`SLT_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=1'b0;
                                end
                                `EXE_SLTU:begin
                                    aluop_o<=`SLTU_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=1'b0;
                                end
                                `EXE_ADD:begin
                                    aluop_o<=`ADD_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=1'b0;
                                end
                                `EXE_SUB:begin
                                    aluop_o<=`SUB_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=1'b0;
                                end
                                default:begin
                                    //op3 over
                                end
                            endcase
                        end
                        default:begin
                            //op2 over
                        end
                    endcase
                end
                `EXE_LUI:begin
                    aluop_o<=`LUI_OP;
                    wreg_o<=1'b1;
                    reg1_read_o<=1'b1;
                    reg2_read_o<=1'b0;
                    instvalid<=1'b0;

                    imm<={inst_i[15:0],16'h0};
                    wd_o<=inst_i[20:16];
                end
                default:begin
                    //op over
                end
            endcase
            if(inst_i[31:21]==11'b00000000000)begin
                //sll、srl、sra 31位-21位均为0
                if(op3==`EXE_SLL)begin
                    aluop_o<=`SLL_OP;
                    wreg_o<=1'b1;
                    reg1_read_o<=1'b0;
                    reg2_read_o<=1'b1;
                    instvalid<=1'b0;

                    imm[4:0]<=inst_i[10:6];
                    wd_o<=inst_i[15:11];
                end else if(op3==`EXE_SRL)begin
                    aluop_o<=`SRL_OP;
                    wreg_o<=1'b1;
                    reg1_read_o<=1'b0;
                    reg2_read_o<=1'b1;
                    instvalid<=1'b0;

                    imm[4:0]<=inst_i[10:6];
                    wd_o<=inst_i[15:11];
                end else if(op3==`EXE_SRA)begin
                    aluop_o<=`SRA_OP;
                    wreg_o<=1'b1;
                    reg1_read_o<=1'b0;
                    reg2_read_o<=1'b1;
                    instvalid<=1'b0;

                    imm[4:0]<=inst_i[10:6];
                    wd_o<=inst_i[15:11];
                end
            end
        end
    end
    
    always @(*) begin
        if(rst==1'b1)begin
            reg1_o<=32'h00000000;
        end else if(reg1_read_o==1'b1)begin
            reg1_o<=reg1_data_i;
        end else if(reg1_read_o==1'b0)begin
            reg1_o<=imm;
        end else begin
            reg1_o<=32'h00000000;
        end
    end

    always @(*) begin
        if(rst==1'b1)begin
            reg2_o<=32'h00000000;
        end else if(reg2_read_o==1'b1)begin
            reg2_o<=reg2_data_i;
        end else if(reg2_read_o==1'b0)begin
            reg2_o<=imm;
        end else begin
            reg2_o<=32'h00000000;
        end
    end

endmodule
```

id_ex模块代码：

```verilog
`timescale 1ns / 1ps

module id_ex(
    input wire rst,
    input wire clk,

    input wire [3:0] id_aluop,
    input wire [31:0] id_reg1,
    input wire [31:0] id_reg2,
    input wire [4:0] id_wd,
    input wire id_wreg,

    output reg [3:0] ex_aluop,
    output reg [31:0] ex_reg1,
    output reg [31:0] ex_reg2,
    output reg [4:0] ex_wd,
    output reg ex_wreg
    );

    always @(posedge clk ) begin
        if(rst==1'b1)begin
            ex_aluop<=`NOP_OP;
            ex_reg1<=32'h00000000;
            ex_reg2<=32'h00000000;
            ex_wd<=5'b00000;
            ex_wreg<=1'b0;
        end else begin
            ex_aluop<=id_aluop;
            ex_reg1<=id_reg1;
            ex_reg2<=id_reg2;
            ex_wd<=id_wd;
            ex_wreg<=id_wreg;
        end
    end
    
endmodule
```

alu模块代码：

```verilog
`timescale 1ns / 1ps
`include "defines.v"
module alu(
    input wire rst,
    input wire [3:0] alu_control,
    input wire [31:0] alu_src1,
    input wire [31:0] alu_src2,
    input wire [4:0] wd_i,//new:目的寄存器地址[信号传递]
    input wire wreg_i,//new:是否要写入目的寄存器[信号传递]
    output reg [31:0] alu_result,
    output reg [4:0] wd_o,//new:目的寄存器地址[信号传递]
    output reg wreg_o//new:是否要写入目的寄存器[信号传递]
    );

    wire [31:0] alu_src2_mux;
    wire [31:0] result_sum;
    wire [31:0] src1_lt_src2;
    //add、sub
    assign alu_src2_mux = ((alu_control==`SUB_OP)||(alu_control==`SLT_OP)) ? (~alu_src2)+1 : alu_src2;
    assign result_sum = alu_src1 + alu_src2_mux;
    //slt、sltu
    assign src1_lt_src2 = (alu_control==`SLT_OP) ?
                          ((alu_src1[31]&&!alu_src2[31]) ||
                           (!alu_src1[31]&&!alu_src2[31]&&result_sum[31]) ||
                           (alu_src1[31]&&alu_src2[31]&&result_sum[31]))
                           //有符号比较，直接将三种为1的情况通过逻辑表达式列出来
                          : (alu_src1<alu_src2);
                          //不是，则为无符号比较，直接输出比较结果
    
    always @(*) begin
        if(rst==1'b1)begin
            alu_result=32'h00000000;
            wd_o=5'b00000;
            wreg_o=1'b0;
        end else begin
            wd_o=wd_i;//new:目的寄存器地址[信号传递]
            wreg_o=wreg_i;//new:是否要写入目的寄存器[信号传递]
            case (alu_control)
                `ADD_OP,`SUB_OP:begin
                    alu_result = result_sum;
                end
                `SLT_OP,`SLTU_OP:begin
                    alu_result = src1_lt_src2;
                end
                //and、nor、or、xor
                `AND_OP:begin
                    alu_result = alu_src1 & alu_src2;
                end
                `NOR_OP:begin
                    alu_result = ~(alu_src1 | alu_src2);
                end
                `OR_OP:begin
                    alu_result = alu_src1 | alu_src2;
                end
                `XOR_OP:begin
                    alu_result = alu_src1 ^ alu_src2;
                end
                //sll、srl、sra
                `SLL_OP:begin
                    alu_result = alu_src2<<alu_src1[4:0];
                end
                `SRL_OP:begin
                    alu_result = alu_src2>>alu_src1[4:0];
                end
                `SRA_OP:begin
                    alu_result = ({32{alu_src2[31]}} << (6'd32-{1'b0,alu_src1[4:0]}))
                    //6'd32-{1'b0,alu_src1[4:0]}表示32-右移位数=当前保留位数
                                | alu_src2>>alu_src1[4:0];
                    //将alu_src2逻辑右移，并将符号位扩展至alu_src1位(符号位左移32-alu_src1位)
                end
                //lui
                `LUI_OP:begin
                    alu_result = alu_src2;
                end
                default:begin
                    alu_result = 32'b0;
                end 
            endcase
        end
    end

endmodule
```

regfile模块代码：

```verilog
`timescale 1ns / 1ps
module regfile(
    input wire rst,
    input wire clk,
    input wire [4:0] waddr,
    input wire [31:0] wdata,
    input wire we,
    input wire [4:0] raddr1,
    input wire re1,
    output reg [31:0] rdata1,
    input wire [4:0] raddr2,
    input wire re2,
    output reg [31:0] rdata2
    );

    reg [31:0] regs[0:31];

    integer i;
    initial begin//new:新增初始化内容
        regs[1]=32'h12345678;
        for(i=2;i<32;i=i+1)begin
            regs[i]=regs[i-1]+32'h01010101;
        end
    end

    always @(posedge clk) begin
        if(rst==1'b0)begin
            if((we==1'b1)&&(waddr!=5'b0))begin
                regs[waddr]<=wdata;
            end
        end
    end

    always @(*) begin
        if(rst==1)begin
            rdata1<=32'h0;
        end else if((raddr1==5'b0)&&(re1==1'b1))begin
            rdata1<=32'h0;
        end else if((raddr1==waddr)&&(we==1'b1)&&(re1==1'b1))begin
            rdata1<=wdata;
        end else if(re1==1'b1)begin
            rdata1<=regs[raddr1];
        end else begin
            rdata1<=32'h0;
        end
    end

    always @(*) begin
        if(rst==1)begin
            rdata2<=32'h0;
            //当复位信号（rst==1）有效时，读数据（rdata1和rdata2）为0
        end else if((raddr2==5'b0)&&(re2==1'b1))begin
            rdata2<=32'h0;
            //当复位信号（rst==0）无效时,当读地址为0，读数据为0
        end else if((raddr2==waddr)&&(we==1'b1)&&(re2==1'b1))begin
            rdata2<=wdata;
            //当读写地址相等，且读写使能都有效的时候，读数据为写数据【数据相关：读后写】
        end else if(re2==1'b1)begin
            rdata2<=regs[raddr2];
            //当读使能有效时，读数据为寄存器堆中存储数据
        end else begin
            rdata2<=32'h0;
        end
    end
    
endmodule
```

ex_mem模块代码：

```verilog
`timescale 1ns / 1ps

module ex_mem(
    input wire rst,
    input wire clk,
    input wire [4:0] ex_wd,
    input wire ex_wreg,
    input wire [31:0] ex_wdata,
    output reg [4:0] mem_wd,
    output reg mem_wreg,
    output reg [31:0] mem_wdata
    );

    always @(posedge clk ) begin
        if(rst==1'b1)begin
            mem_wd<=5'b00000;
            mem_wreg<=1'b0;
            mem_wdata<=32'h00000000;
        end else begin
            mem_wd<=ex_wd;
            mem_wreg<=ex_wreg;
            mem_wdata<=ex_wdata;
        end
    end

endmodule
```

mem模块代码：

```verilog
`timescale 1ns / 1ps

module mem(
    input wire rst,
    input wire [4:0] wd_i,
    input wire wreg_i,
    input wire [31:0] wdata_i,
    output reg [4:0] wd_o,
    output reg wreg_o,
    output reg [31:0] wdata_o
    );

    always @(*) begin
        if(rst==1'b1)begin
            wd_o<=5'b00000;
            wreg_o<=1'b0;
            wdata_o<=32'h00000000;
        end else begin
            wd_o<=wd_i;
            wreg_o<=wreg_i;
            wdata_o<=wdata_i;
        end
    end

endmodule
```

mem_wb模块代码：

```verilog
`timescale 1ns / 1ps

module mem_wb(
    input wire rst,
    input wire clk,
    input wire [4:0] mem_wd,
    input wire mem_wreg,
    input wire [31:0] mem_wdata,
    output reg [4:0] wb_wd,
    output reg wb_wreg,
    output reg [31:0] wb_wdata
    );

    always @(posedge clk ) begin
        if(rst==1'b1)begin
            wb_wd<=5'b00000;
            wb_wreg<=1'b0;
            wb_wdata<=32'h00000000;
        end else begin
            wb_wd<=mem_wd;
            wb_wreg<=mem_wreg;
            wb_wdata<=mem_wdata;
        end
    end

endmodule
```

inst_rom模块代码：

```verilog
`timescale 1ns / 1ps
module inst_rom(
    input wire ce,
    input wire [31:0] addr,
    output reg [31:0] inst
    );

    reg [31:0] inst_mem[0:131071];
    initial $readmemh("D:/language/Verilog/pipeline_cpu/pipeline_cpu.srcs/sources_1/new/inst_rom.data",inst_mem);
    always @(*) begin
        if(ce==1'b0)begin
            inst<=32'b0;
        end else begin
            inst<=inst_mem[addr[18:2]];
        end
    end
    
endmodule
```

defines模块代码：

```verilog
`define ADD_OP 4'b0000
`define SUB_OP 4'b0001
`define SLT_OP 4'b0010
`define SLTU_OP 4'b0011
`define AND_OP 4'b0100
`define NOR_OP 4'b0101
`define OR_OP 4'b0110
`define XOR_OP 4'b0111
`define SLL_OP 4'b1000
`define SRL_OP 4'b1001
`define SRA_OP 4'b1010
`define LUI_OP 4'b1011
`define NOP_OP 4'b1111

`define EXE_AND  6'b100100
`define EXE_OR   6'b100101
`define EXE_XOR 6'b100110
`define EXE_NOR 6'b100111
`define EXE_LUI 6'b001111

`define EXE_SLL  6'b000000
`define EXE_SRL  6'b000010
`define EXE_SRA  6'b000011

`define EXE_SLT  6'b101010
`define EXE_SLTU  6'b101011

`define EXE_ADD  6'b100000
`define EXE_SUB  6'b100010
`define EXE_SPECIAL_INST 6'b000000
```

***

下列是对之前完成模块的封装。



pipeline_cpu模块代码：

将pc模块、id模块、alu模块、regfile模块、mem模块、if_id模块，id_ex模块，ex_mem模块，mem_wb模块例化，并进行信号的连接

大量修改：包括但不限于添加新变量、新模块、增加新连接

```verilog
`timescale 1ns / 1ps

module pipeline_cpu(
    input wire clk,
    input wire rst,
    input wire [31:0] rom_data_i,
    output wire [31:0] rom_addr_o,
    output wire rom_ce_o
    );

    pc_reg pc_reg0(.rst(rst),
                   .clk(clk),
                   .pc(rom_addr_o),
                   .ce(rom_ce_o));

    //if_id ~ id
    wire [31:0] id_inst_i;//new

    //id ~ id_ex
    wire [3:0] id_aluop_o;
    wire [31:0] id_reg1_o;
    wire [31:0] id_reg2_o;
    wire [4:0] id_wd_o;
    wire id_wreg_o;

    //id_ex ~ ex
    wire [3:0] ex_aluop_i;//new
    wire [31:0] ex_reg1_i;//new
    wire [31:0] ex_reg2_i;//new
    wire [4:0] ex_wd_i;//new
    wire ex_wreg_i;//new

    //ex ~ ex_mem
    wire [31:0] ex_wdata_o;//new
    wire [4:0] ex_wd_o;//new
    wire ex_wreg_o;//new

    //ex_mem ~ mem
    wire [31:0] mem_wdata_i;//new
    wire [4:0] mem_wd_i;//new
    wire mem_wreg_i;//new

    //mem ~ mem_wb
    wire [31:0] mem_wdata_o;//new
    wire [4:0] mem_wd_o;//new
    wire mem_wreg_o;//new
    
    //mem_wb ~ regfile
    wire [31:0] wb_wdata_i;//new
    wire [4:0] wb_wd_i;//new
    wire wb_wreg_i;//new

    //id ~ regfile
    wire reg1_read;
    wire reg2_read;
    wire [4:0] reg1_addr;
    wire [4:0] reg2_addr;
    wire [31:0] reg1_data;
    wire [31:0] reg2_data;

    if_id if_id0(//new
        .rst(rst),
        .clk(clk),
        .if_inst(rom_data_i),.id_inst(id_inst_i)
    );

    id id0(.rst(rst),.inst_i(id_inst_i),
           .reg1_data_i(reg1_data),.reg2_data_i(reg2_data),
           .reg1_read_o(reg1_read),.reg2_read_o(reg2_read),
           .reg1_addr_o(reg1_addr),.reg2_addr_o(reg2_addr),
           .aluop_o(id_aluop_o),
           .reg1_o(id_reg1_o),.reg2_o(id_reg2_o),
           .wd_o(id_wd_o),.wreg_o(id_wreg_o)
    );

    //删除：
    //wire [31:0] wdata_o;
    //wire [4:0] wd_o;
    //wire wreg_o;

    id_ex id_ex0(//new
        .rst(rst),
        .clk(clk),
        .id_aluop(id_aluop_o),.id_reg1(id_reg1_o),.id_reg2(id_reg2_o),
        .id_wd(id_wd_o),.id_wreg(id_wreg_o),
        .ex_aluop(ex_aluop_i),.ex_reg1(ex_reg1_i),.ex_reg2(ex_reg2_i),
        .ex_wd(ex_wd_i),.ex_wreg(ex_wreg_i)
    );

    alu alu0(
        .rst(rst),
        .alu_control(ex_aluop_i),
        .alu_src1(ex_reg1_i),.alu_src2(ex_reg2_i),
        .wd_i(ex_wd_i),.wreg_i(ex_wreg_i),
        .alu_result(ex_wdata_o),//new
        .wd_o(ex_wd_o),.wreg_o(ex_wreg_o)//new
    );

    ex_mem ex_mem0(//new
        .rst(rst),
        .clk(clk),
        .ex_wd(ex_wd_o),.ex_wreg(ex_wreg_o),.ex_wdata(ex_wdata_o),
        .mem_wd(mem_wd_i),.mem_wreg(mem_wreg_i),.mem_wdata(mem_wdata_i)
    );

    mem mem0(//new
        .rst(rst),
        .wd_i(mem_wd_i),.wreg_i(mem_wreg_i),.wdata_i(mem_wdata_i),
        .wd_o(mem_wd_o),.wreg_o(mem_wreg_o),.wdata_o(mem_wdata_o)
    );

    mem_wb mem_wb0(//new
        .rst(rst),
        .clk(clk),
        .mem_wd(mem_wd_o),.mem_wreg(mem_wreg_o),.mem_wdata(mem_wdata_o),
        .wb_wd(wb_wd_i),.wb_wreg(wb_wreg_i),.wb_wdata(wb_wdata_i)
    );

    regfile regfile0(
        .rst(rst),.clk(clk),
        .waddr(wb_wd_i),.wdata(wb_wdata_i),.we(wb_wreg_i),//new
        .raddr1(reg1_addr),.re1(reg1_read),
        .rdata1(reg1_data),
        .raddr2(reg2_addr),.re2(reg2_read),
        .rdata2(reg2_data)
    );
    
endmodule
```

mips_sopc模块：

将pipeline_cpu模块、inst_rom模块例化，并进行信号的连接

```verilog
`timescale 1ns / 1ps
module mips_sopc(
    input wire clk,
    input wire rst
    );

    wire [31:0] inst_addr;
    wire [31:0] inst;
    wire rom_ce;

    pipeline_cpu pipeline_cpu0(//new:仅修改例化名即可
        .clk(clk),.rst(rst),
        .rom_data_i(inst),
        .rom_addr_o(inst_addr),
        .rom_ce_o(rom_ce)
    );

    inst_rom inst_rom0(
        .ce(rom_ce),
        .addr(inst_addr),
        .inst(inst)
    );
    
endmodule
```

mips_sopc_tb模块：

传递时钟信号(clk)和复位信号(rst)，例化mips_sopc，是mips最顶层的文件

```verilog
`timescale 1ns / 1ps
module mips_sopc_tb(
);

    reg clk;
    reg rst;
    initial begin
        clk=1'b0;
        forever #10 clk=~clk;
    end
    initial begin
        rst=1;
        #100 rst=0;
        #1000 $stop;
    end
    mips_sopc mips_sopc0(
        .clk(clk),
        .rst(rst)
    );
endmodule
```

激励文件：

这里的激励文件为通过inst_rom模块将data文件中的数据进行读入和mips_sopc_tb模块顶层输入的时钟信号(clk)和复位信号(rst)

inst_rom.data文件：【目测可以运行译码器、单周期CPU和多周期CPU，主要实现12条MIPS指令】

```txt
00411820
00412022
00412825
00413024
00413826
00414027
0022182A
0041202B
00024A00
00055202
00065A03
3C06FEDC
```

仿真结果如下图所示：





总结一下，流水线就是插入寄存器，以面积换取速度。 

> 流水线的逐级反压串扰

解决这个问题的办法就是切割这种逐级握手机制。

+ 使用乒乓buffer：[芯片设计小经验——乒乓buffer](https://www.eda365.com/thread-452542-1-1.html)
+ 使用旁路缓存Cache Aside：[缓存一致性之Cache Aside（旁路缓存）模式](https://www.xttblog.com/?p=4478)

> + 乒乓buffe

又叫double buffer，由两块同样大小的memory组成，一乒一乓。放在数据通路的中间，在大部分时候都能保证一块memory收上游的数据，一块memory往下游发数据，一读一写并行操作。

乒乓buffer主要应用在以下场景进行带宽的提升：

- 下游必须等到上游数据全部写完或者积累到某个程度才能开始读
- 上游必须等到下游数据全部读完或者读到某个程度才能开始写



关注公众号【数字IC自修室】



> + 旁路缓存Cache Aside

[缓存读写策略-Cache Aside（旁路缓存）策略](https://www.pianshen.com/article/63512004914/)

缓存模式，一般有 3 种：Cache Aside（旁路缓存）、Read/Write Through（读写穿透）、Write Behind Caching（异步缓存写入）。

[常见缓存读写策略](https://blog.csdn.net/qq_39340792/article/details/112383445?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_ecpm_v1~rank_aggregation-11-112383445.pc_agg_rank_aggregation&utm_term=%E6%97%81%E8%B7%AF%E7%BC%93%E5%AD%98%E7%AD%96%E7%95%A5&spm=1000.2123.3001.4430)

缓存模式，简单来说就是利用时间局限性原理，通过空间换时间来达到加速数据获取的目的。

Cache Aside（旁路缓存）策略以数据库中的数据为准，缓存中的数据是按需加载的。它可以分为读策略和写策略。

**读策略**
 从缓存中读取数据；如果缓存命中，则直接返回数据；如果缓存不命中，则从数据库中查询数据；查询到数据后，将数据写入到缓存中，并且返回给用户。

**写策略**
 更新数据库中的记录；删除缓存记录。

> 有关流水线性能的若干问题

+ 流水线并不能减少(而且一般是增加)单条指令的执行时间，但却能提高吞吐率

+ 增加流水线的深度(段数)可以提供流水线的性能

+ 流水线的深度受限于流水线的延迟和流水线的额外开销

  + 流水线的额外开销包括：
  + 流水寄存器的延迟(建立时间和传输延迟)
  + 时钟扭曲

+ 当时钟周期小到与额外开销相同时流水已没意义。因为这时在每一个时钟周期中已没有时间来做有用的工作

+ 需要高速锁存器来作为流水寄存器。

  Earle锁存器：

  + 对时钟扭曲不太敏感(相对而言)
  + 其延迟为常数：2个门级延迟避免了数据通过锁存器时的扭曲
  + 锁存器中可以进行两级逻辑运算而不增延迟时间。这样每个流水段中的两级逻辑可以与锁存器重叠，从而能隐藏锁存器开销的绝大部分

+ 相关问题：结构相关、数据相关、控制相关

> 流水线危害 流水线问题

[流水线架构_流水线问题（危害） 计算机架构](https://blog.csdn.net/cumt30111/article/details/107803462)

 通常，主要有三种**类型的危害** ： 

1. Structural Hazard 结构危害 
2. Data Hazard 资料危害 
3. Control Hazard 控制危害 

**结构危害**：当我们尝试在同一时钟周期内使用同一硬件执行多个或两个不同的操作时，这会阻止管道正常工作，这被称为结构危害。 

例如：数据和指令存在同一存储器中时，访存指令会导致访存冲突

解决办法：

插入暂停周期（“流水线气泡”或“气泡”）

+ 为了避免这种情况，处理器可以在流水线中使用<font color=FF0000>停顿</font>。一个周期的停顿将使流水线转移到一个时钟周期，直到可以完全避免或消除危害为止。

设置相互独立的指令存储器和数据存储器或设置相互独立的指令Cache和数据Cache

+ 如果我们有<font color=FF0000>单独的数据缓存和指令缓存</font>，则不会发生这种情况或危害。 

避免结构相关：【硬件代价很大】

1. 所有功能单元完全流水化
2. 设置足够的硬件资源

***

**数据危害**： 在发生数据危险时，流水线中不同指令对共享变量的读写操作可能导致不同类型的数据依赖关系，例如， 

1. Read after write hazard 写后读取危险 
2. Write after reading hazard 阅读危险后写 
3. Write after write hazard 写后写危险

当一条指令取决于上一条指令的结果时，它们就会出现，即通过流水线中指令的重叠来暴露。为了避免数据危害，我们可以做：

定向技术

+ Internal forwarding 内部转发：为避免写危险后读取的情况，放置在ALU单元之后的锁存器将用作内部转发，以将输出到下一条指令的输出传输到放置在解码单元之前的锁存器。

停顿、后推法、暂停

+ Stalling 失速 ：增加流水线互锁硬件，插入“暂停”，直到相关消失

**注意：**这些是与数据危害有关的其他一些重要术语： 

1. **Data dependency** **数据依赖**
2. **Flow dependency** **流依赖性**：寄存器R1由I1加载，然后由I2使用。 因此，执行指令后一个寄存器的结果可能会影响该寄存器的操作
3. **Output dependency** **输出依赖性**：当两条指令要同时写入时

对数据相关的编译调度方法：

`流水线调度、指令调度`：编译器通过重新排列代码顺序来消除某些暂停

指令发射issue：指令从译码阶段ID进入执行阶段EX

在ID阶段，

+ 检测所有的数据相关。若数据相关，则在指令流出前让其暂停
+ 确定需要什么样的定向，并设置相应的控制

也可以在需要用到操作数的那个时钟周期检测相关或定向

例如：

由Load指令引起的RAW相关的互锁(Load互锁)可以通过ID段的检测来实现

![2021-11-20-hazard](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-11-20-hazard.png)

到ALU输入的定向可以在EX段实现

![2021-11-20-hazard2](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-11-20-hazard2.png)

***

**控制危险**：当我们需要找到任何分支指令的主根时，就会发生控制危险，并且在到达该指令的根之前，不能继续进行另一条指令。 

一般是分支指令和其他能够改变PC值的指令

分支指令执行结果：

+ 转移失败，PC+4
+ 转移成功，将PC值改变为转移目标地址【到MEM阶段的末尾才改变】

解决方法：

暂停：一旦检测到分支指令【ID段】，就暂停执行其后的指令，知道分支指令到达MEM段，确定出新的PC值为止

+ 分支转移成功导致暂停三个时钟周期

分支预测

+ 当分支方向猜测错误，不仅流水线中有多个流水段要浪费，更严重的是可能造成程序执行结果发生错误

  方案：

  1.只进行指令译码和准备好运算所需要的操作数，在转移条件没有形成之前不执行运算

  【造成流水线流水段的浪费，但控制逻辑比较简单】

  2.一直执行到运算完成，但不送回运算结果

  【设置一定数量的缓冲寄存器来存放中间运算结果，控制逻辑比较复杂】

延迟槽、延迟分支delayed branch：

+ 若预测方向正确，正常执行延迟槽中的指令；否则将之变为nop指令

+ 分支延迟指令：在延迟槽中放入有用的指令

+ 三种调度方法：
  + 从前调度（最好）
  + 从目标处调度
  + 从失败处调度

***



***

**流水线危害**：见上述描述，即数据相关、控制相关、结构相关

**管道危险：仅仅是任何障碍，状况，或者可以说是任何阻碍管道正常工作或行为的情况。**



## 指令实现

### 算术运算指令

加减法指令【代码见**判断溢出**】

+ ADD、ADDI、SUB：判断溢出，产生溢出异常，同时不保存结果
+ ADDU、ADDIU、SUBU：不进行溢出检查

乘除法指令【代码见**乘法器、除法器**】

+ MUL、MULT、MULTU
  + MUL：有符号相乘，结果低32位保存到通用寄存器
  + MULT、MULTU：MULT有符号相乘、MULTU无符号相乘，结果保存到hilo寄存器
+ DIV、DIVU

alu.v

```verilog
				`MUL_OP:begin
                    alu_result=mul_result_i[31:0];
                end
                `MULT_OP,`MULTU_OP:begin
                    whilo_o=1'b1;
                    hi_o=mul_result_i[63:32];
                    lo_o=mul_result_i[31:0];
                end
                `DIV_OP,`DIVU_OP:begin
                    whilo_o=1'b1;
                    hi_o=div_result_i[63:32];
                    lo_o=div_result_i[31:0];
                end
```

计数运算

+ CLO：coun_leading_ones，计算遇到第一个0之前，1的个数
+ CLZ：coun_leading_zeros，计算遇到第一个1之前，0的个数

id.v

```verilog
			//6'b011100
			`EXE_SPECIAL2_INST:begin//op
                	case (op2)
                        5'b00000:begin
                            case(op3)
                                `EXE_CLZ:begin//clz
                                    wreg_o<=`WriteEnable;
                                    aluop_o<=`CLZ_OP;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b0;
                                    instvalid<=`InstValid;
                                end
                                `EXE_CLO:begin//clo
                                    wreg_o<=`WriteEnable;
                                    aluop_o<=`CLO_OP;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b0;
                                    instvalid<=`InstValid;
                                end
                                default:begin
                                    //op3 over
                                end
                    		endcase
                        end
                        default:begin
                            //op2 over
                        end
                    endcase//EXE_SPECIAL_INST2 case
                end
				default:begin
                    //op over
                end
```

alu.v

```verilog
	wire[31:0] reg1_i_not;
	assign reg1_i_not=~reg1_i;

			case(aluop_i)
				`EXE_CLZ_OP:begin
                    alu_result<=reg1_i[31] ? 0
                    					:reg1_i[30] ? 1
                    					:reg1_i[29] ? 2
                    					:reg1_i[28] ? 3
                    					:reg1_i[27] ? 4
                    					:reg1_i[26] ? 5
                    					:reg1_i[25] ? 6
                    					:reg1_i[24] ? 7
                    					:reg1_i[23] ? 8
                    					:reg1_i[22] ? 9
                    					:reg1_i[21] ? 10
                    					:reg1_i[20] ? 11
                    					:reg1_i[19] ? 12
                    					:reg1_i[18] ? 13
                    					:reg1_i[17] ? 14
                    					:reg1_i[16] ? 15
                    					:reg1_i[15] ? 16
                    					:reg1_i[14] ? 17
                    					:reg1_i[13] ? 18
                    					:reg1_i[12] ? 19
                    					:reg1_i[11] ? 20
                    					:reg1_i[10] ? 21
                    					:reg1_i[9] ? 22
                    					:reg1_i[8] ? 23
                    					:reg1_i[7] ? 24
                    					:reg1_i[6] ? 25
                    					:reg1_i[5] ? 26
                    					:reg1_i[4] ? 27
                    					:reg1_i[3] ? 28
                    					:reg1_i[2] ? 29
                    					:reg1_i[1] ? 30
                    					:reg1_i[0] ? 31:32;
                end
                `EXE_CLO_OP:begin
                    alu_result<=reg1_i_not[31] ? 0
                                 		:reg1_i_not[30] ? 1
                                 		:reg1_i_not[29] ? 2
                                 		:reg1_i_not[28] ? 3
                                 		:reg1_i_not[27] ? 4
                                 		:reg1_i_not[26] ? 5
                                 		:reg1_i_not[25] ? 6
                    					:reg1_i_not[24] ? 7
                    					:reg1_i_not[23] ? 8
                    					:reg1_i_not[22] ? 9
                    					:reg1_i_not[21] ? 10
                                 		:reg1_i_not[20] ? 11
                    					:reg1_i_not[19] ? 12
                    					:reg1_i_not[18] ? 13
                    					:reg1_i_not[17] ? 14
                    					:reg1_i_not[16] ? 15
                                 		:reg1_i_not[15] ? 16
                    					:reg1_i_not[14] ? 17
                    					:reg1_i_not[13] ? 18
                    					:reg1_i_not[12] ? 19
                    					:reg1_i_not[11] ? 20
                                 		:reg1_i_not[10] ? 21
                    					:reg1_i_not[9] ? 22
                    					:reg1_i_not[8] ? 23
                    					:reg1_i_not[7] ? 24
                    					:reg1_i_not[6] ? 25
                                 		:reg1_i_not[5] ? 26
                    					:reg1_i_not[4] ? 27
                    					:reg1_i_not[3] ? 28
                    					:reg1_i_not[2] ? 29
                    					:reg1_i_not[1] ? 30
                                 		:reg1_i_not[0] ? 31:32;
                end
			endcase
```



乘累加、乘累减

+ madd、msub：有符号乘法 {hi,lo} ± reg1×reg2
+ maddu、msubu：无符号乘法 {hi,lo} ± reg1×reg2
  + 第一个时钟周期：乘法运算reg1×reg2
  + 第二个时钟周期：乘法结果与hilo寄存器进行加减法

id.v

```verilog
			//6'b011100
			`EXE_SPECIAL2_INST:begin//op
                	case (op2)
                        5'b00000:begin
                            case(op3)
                                `EXE_MUL:begin//mul
                                    wreg_o<=`WriteEnable;
                                    aluop_o<=`MUL_OP;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=`InstValid;
                                end
                                `EXE_MADD:begin//madd
                                    wreg_o<=`WriteDisable;
                                    aluop_o<=`MADD_OP;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=`InstValid;
                                end
                                `EXE_MADDU:begin//maddu
                                    wreg_o<=`WriteDisable;
                                    aluop_o<=`MADDU_OP;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=`InstValid;
                                end
                                `EXE_MSUB:begin//msub
                                    wreg_o<=`WriteDisable;
                                    aluop_o<=`MSUB_OP;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=`InstValid;
                                end
                                `EXE_MSUBU:begin//msubu
                                    wreg_o<=`WriteDisable;
                                    aluop_o<=`MSUBU_OP;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=`InstValid;
                                end
                                default:begin
                                    //op3 over
                                end
                    		endcase
                        end
                        default:begin
                            //op2 over
                        end
                    endcase//EXE_SPECIAL_INST2 case
                end
				default:begin
                    //op over
                end
```

alu.v

```verilog
`timescale 1ns / 1ps
`include "defines.v"
module alu(
	//...
    
	//...
    );

			case(aluop_i)
                `MADD_OP,`MADDU_OP:begin
                    if(cnt_i==2'b00)begin
                        hilo_temp_o<=mulres;
                        cnt_o<=2'b01;
                        hilo_temp1<={`ZeroWord,`ZeroWord};
                        stallreq_for_madd_msub<=`Stop;
                    end else if(cnt_i==2'b01)begin
                        hilo_temp_o<=mulres;
                        cnt_o<=2'b10;
                        hilo_temp1<=hilo_temp_i+{HI,LO};
                        stallreq_for_madd_msub<=`NoStop;
                    end
                end
                `MSUB_OP,`MSUBU_OP:begin
                    if(cnt_i==2'b00)begin
                        hilo_temp_o<=~mulres+1;
                        cnt_o<=2'b01;
                        stallreq_for_madd_msub<=`Stop;
                    end else if(cnt_i==2'b01)begin
                        hilo_temp_o<={`ZeroWord,`ZeroWord};
                        cnt_o<=2'b10;
                        hilo_temp1<=hilo_temp_i+{HI,LO};
                        stallreq_for_madd_msub<=`NoStop;
                    end
                end
                default:begin
                    hilo_temp_o<={`ZeroWord,`ZeroWord};
                    cnt_o<=2'b00;
                    stallreq_for_madd_msub<=`NoStop;
                end
            endcase
```



### 比较运算指令

SLT

SLTI：有符号比较

SLTIU：无符号比较

### 逻辑运算指令

and、or、xor、nor

andi、xori：立即数零扩展后与reg1运算，将结果保存至reg2通用寄存器中

lui：将立即数高16位加载，将结果保存至reg2通用寄存器中

### 移位操作指令

sll、srl、sra：移位位数 <font color=ff0000>sa</font> 位

sllv、srlv、srav：移位位数 <font color=ff0000>通用寄存器0-4bit的值 </font>位

> left & right ：l左移、r右移
>
> logic & arithmetic ：l逻辑[0填充]、a算术[最高位符号位填充]



id.v

```verilog
			case (op)
				`EXE_SPECIAL_INST:begin
                    case(op2)
                        5'b00000:begin
                            case(op3)
                                `EXE_SLLV:begin
                                    aluop_o<=`SLL_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=1'b0;
                                end
                                `EXE_SRLV:begin
                                    aluop_o<=`SRL_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=1'b0;
                                end
                                `EXE_SRAV:begin
                                    aluop_o<=`SRA_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=1'b0;
                                end
                                default:begin
                                    //op3 over
                                end
                            endcase
                        end
                        default:begin
                            //op2 over
                        end
                    endcase
                end
				default:begin
                    //op over
                end
            endcase
			if(inst_i[31:21]==11'b00000000000)begin
                //sll、srl、sra 31位-21位均为0
                if(op3==`EXE_SLL)begin
                    aluop_o<=`SLL_OP;
                    wreg_o<=1'b1;
                    reg1_read_o<=1'b0;
                    reg2_read_o<=1'b1;
                    instvalid<=1'b0;

                    imm[4:0]<=inst_i[10:6];
                    wd_o<=inst_i[15:11];
                end else if(op3==`EXE_SRL)begin
                    aluop_o<=`SRL_OP;
                    wreg_o<=1'b1;
                    reg1_read_o<=1'b0;
                    reg2_read_o<=1'b1;
                    instvalid<=1'b0;

                    imm[4:0]<=inst_i[10:6];
                    wd_o<=inst_i[15:11];
                end else if(op3==`EXE_SRA)begin
                    aluop_o<=`SRA_OP;
                    wreg_o<=1'b1;
                    reg1_read_o<=1'b0;
                    reg2_read_o<=1'b1;
                    instvalid<=1'b0;

                    imm[4:0]<=inst_i[10:6];
                    wd_o<=inst_i[15:11];
                end
            end
```



### 空指令

nop、ssnop：特殊的逻辑左移指令，不用特意实现

+ nop = sll \$0,\$0,0
+ ssnop = sll \$0,\$0,1

sync、pref



id.v

```verilog
				`EXE_SPECIAL_INST:begin
                    case(op2)
                        5'b00000:begin
                            case(op3)
                                `EXE_SYNC:begin
                                    wreg_o<=`WriteDisable;
                                    aluop_o<=`NOP_OP;
                                    reg1_read_o<=1'b0;
                                    reg2_read_o<=1'b1;
                                    instvalid<=`InstValid;
                                end
                                default:begin
                                end
                            endcase
                        end
                        default:begin
                        end
                    endcase
                end
				`EXE_PREF:begin
                    wreg_o<=`WriteDisable;
                    aluop_o<=`NOP_OP;
                    reg1_read_o<=1'b0;
                    reg2_read_o<=1'b0;
                    instvalid<=`InstValid;
                end
```



### 移动操作指令

判断rt通用寄存器值是否为零：movn、movz

hilo寄存器访问指令：mfhi、mflo、mthi、mtlo【具体见hilo寄存器】

协处理器访问指令：mtc0、mfc0【具体见cp0协处理器】



> mt：通用寄存器赋值给特殊寄存器
>
> mf：特殊寄存器赋值给通用寄存器

id.v

```verilog
				case (op)
				`EXE_SPECIAL_INST:begin
                    case(op2)
                        5'b00000:begin
                            case(op3)
								`EXE_MFHI:begin//对HI、LO寄存器的读取
                                    aluop_o<=`MFHI_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b0;
                                    reg2_read_o<=1'b0;
                                    instvalid<=1'b0;
                                end
                                `EXE_MFLO:begin
                                    aluop_o<=`MFLO_OP;
                                    wreg_o<=1'b1;
                                    reg1_read_o<=1'b0;
                                    reg2_read_o<=1'b0;
                                    instvalid<=1'b0;
                                end
                                `EXE_MTHI:begin//对HI、LO寄存器的写入，指令格式中目的寄存器为0，即wd_o=0
                                    aluop_o<=`MTHI_OP;
                                    wreg_o<=1'b0;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b0;
                                    instvalid<=1'b0;
                                end
                                `EXE_MTLO:begin
                                    aluop_o<=`MTLO_OP;
                                    wreg_o<=1'b0;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b0;
                                    instvalid<=1'b0;
                                end
								`EXE_MOVN:begin
                                    aluop_o<=`MOVN_OP;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=`InstValid;
                                    //reg2_o不等于0，将reg1的通用寄存器值赋给地址为wd_o的通用寄存器
                                    if(reg2_o!=`ZeroWord) begin
                                        wreg_o<=`WriteEnable;
                                    end else begin
                                        wreg_o<=`WriteDisable;
                                    end
                                end
                                `EXE_MOVZ:begin
                                    aluop_o<=`MOVZ_OP;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b1;
                                    instvalid<=`InstValid;
                                    //reg2_o等于0，将reg1的通用寄存器值赋给地址为wd_o的通用寄存器
                                    if(reg2_o==`ZeroWord) begin
                                        wreg_o<=`WriteEnable;
                                    end else begin
                                        wreg_o<=`WriteDisable;
                                    end
                                end
                                default:begin
                                    //op3 over
                                end
                            endcase
                        end
                        default:begin
                            //op2 over
                        end
                    endcase
                end
				default:begin
                    //op over
                end
            endcase
			if(inst_i==`EXE_ERET) begin//eret
                wreg_o<=`WriteDisable;
                aluop_o<=`ERET_OP;
                reg1_read_o<=1'b0;
                reg2_read_o<=1'b0;
                instvalid<=`InstValid;
                excepttype_is_eret<=`True_v;
            end else if(inst_i[31:21]==11'b01000000000 && inst_i[10:0]==11'b00000000000) begin//mfc0
                aluop_o<=`MFC0_OP;
                reg1_read_o<=1'b0;
                reg2_read_o<=1'b0;
                wd_o<=inst_i[20:16];
                wreg_o<=`WriteEnable;
                instvalid<=`InstValid;
            end else if(inst_i[31:21]==11'b01000000100 && inst_i[10:0]==11'b00000000000) begin//mtc0
                aluop_o<=`MTC0_OP;
                reg1_read_o<=1'b1;
                reg2_read_o<=1'b0;
                reg1_addr_o<=inst_i[20:16];
                wd_o<=inst_i[20:16];
                wreg_o<=`WriteDisable;
                instvalid<=`InstValid;
            end
```

alu.v

```verilog
			case (aluop_i)
                //mfhi、mflo
                `MFHI_OP:begin
                    alu_result<=HI;
                end
                `MFLO_OP:begin
                    alu_result<=LO;
                end
                //mthi、mtlo
                `MTHI_OP:begin
                    whilo_o=1'b1;
                    hi_o=alu_src1;
                    lo_o=LO;//写HI寄存器，LO寄存器保持不变
                end
                `MTLO_OP:begin
                    whilo_o=1'b1;
                    hi_o=HI;
                    lo_o=alu_src1;//写LO寄存器，HI寄存器保持不变
                end
                //movz、movn
                `MOVZ_OP:begin
                    alu_result<=alu_src1;
                end
                `MOVN_OP:begin
                    alu_result<=alu_src1;
                end
                //mfc0
                `MFC0_OP:begin
                    cp0_reg_read_addr_o<=inst_i[15:11];
                    alu_result<=cp0_reg_data_i;
                    //cp0数据相关问题
                    if(mem_cp0_reg_we==`WriteEnable && mem_cp0_reg_write_addr==cp0_reg_read_addr_o) begin
                        alu_result<=mem_cp0_reg_data;
                    end else if(wb_cp0_reg_we==`WriteEnable && wb_cp0_reg_write_addr==cp0_reg_read_addr_o) begin
                        alu_result<=wb_cp0_reg_data;
                    end
                end
                //mtc0
                `MTC0_OP:begin
                    cp0_reg_write_addr_o<=inst_i[15:11];
                    cp0_reg_we_o<=1'b1;
                    cp0_reg_data_o<=alu_src1;
                end
                default:begin
                    whilo_o=1'b0;
                    hi_o=32'h00000000;
                    lo_o=32'h00000000;
                    
                    cp0_reg_write_addr_o<=5'b00000;
                    cp0_reg_we_o<=1'b0;
                    cp0_reg_data_o<=32'h00000000;
                end
            endcase
```



### 分支跳转指令

跳转指令【无条件跳转、绝对转移】

+ j、jal：转移地址 低26位为<font color=ff0000>立即数instr_index左移2位</font>，高4位为<font color=ff0000>延迟槽指令的地址高4位</font>

+ jr、jalr：转移地址 <font color=ff0000>通用寄存器的值</font>

+ > al 将跳转指令后面的第二条指令的地址作为返回地址保存到目的通用寄存器
  >
  > 若没有在指令中指明目的寄存器，默认将返回地址保存到寄存器\$31

分支指令【有条件跳转、相对转移】

+ 直接根据指令中的指令码判断指令：
+ b
+ beq、bne
+ bgtz、blez
+ 指令码是REGIMM(6'b000001)，根据16-20bit的值判断指令
+ bal
+ bgez、bgezal
+ bltz、bltzal

> 转移目标地址：offset(0-15bit)左移2位【符号扩展至32位】 + 延迟槽指令地址

【具体代码见延迟槽】



### 加载存储指令

加载指令

+ lb、lh、lw
+ lbu、lhu
+ lwl、lwr：非对齐加载，向左/右

存储指令

+ sb、sh、sw
+ swl、swr：非对齐存储，向左/右

特殊的加载存储指令【用来实现信号量机制】——>这部分涉及到一些操作系统的知识点【进程管理】

+ ll：LLbit=1
+ sc：LLbit=0



> 加载存储指令实现

1. 添加数据存储器data_ram.v
2. mips_sopc模块中例化数据存储器，并将信号连接
3. pipeline_cpu.v中将数据存储器传入的信号连接入mem(访存)模块
4. id、ex模块将 mem_addr_o内存指定地址、store_src_o通用寄存器的值、control_o运算指令 信号传递进入mem模块
5. mem模块，即访存阶段实现加载存储指令

【雷思磊这本书中访存实现是在当前周期写入写出；但，比赛实际中模拟真正的CPU访存模式，访存信号应该提前一个周期传入data_ram，即需要在ex模块将访存信号传出，在mem模块将数据取回】





load相关问题





>ll、sc指令实现
>
>回写阶段新增LLbit.v模块，存储链接状态位
>
>在回写阶段写LLbit寄存器；将LLbit寄存器的值传递到访存阶段供指令sc判断

LLbit.v模块

```verilog
`timescale 1ns / 1ps
`include "defines.v"

module LLbit_reg(
    input wire clk,
    input wire rst,

    input wire flush,
    
    input wire LLbit_i,
    input wire we,

    output reg LLbit_o
    
    );
    
    always @ (posedge clk) begin
        if(rst==`RstEnable) begin
            LLbit_o<=1'b0;
        end else if((flush==1'b1)) begin
            LLbit_o<=1'b0;
        end else if((we==1'b1)) begin
            LLbit_o<=LLbit_i;
        end
    end
endmodule

```



### 异常相关指令

自陷指令：

+ 不包含立即数：
  + teq、tne
  + tge、tgeu
  + tlt、tltu

+ 包含立即数：
  + teqi、tnei
  + tgei、tgeiu
  + tlti、tltiu

系统调用指令

+ syscall

异常返回指令

+ eret

## 关键技术分析与设计

### 判断溢出

加减法指令(ADD、ADDI、SUB)：修改ex阶段

+ 通过“两个加数均为正，结果为负；两个加数均为负，结果为正”判断，这里减法中的减一个操作数转换为加该操作数的负数



alu.v

```verilog
	//add、sub
    wire [31:0] alu_src2_mux;
    wire [31:0] result_sum;
    assign alu_src2_mux = ((alu_control==`SUB_OP)||
                           (alu_control==`SUBU_OP)||
                           (alu_control==`SLT_OP)) ? (~alu_src2)+1 : alu_src2;
    assign result_sum = alu_src1 + alu_src2_mux;
    //判断溢出
    //正数之和为负数，负数之和为正数
    wire ov_sum;//保存溢出情况
    assign ov_sum = (((!alu_src1[31])&&(!alu_src2_mux[31]))&&result_sum[31])||((alu_src1[31]&&alu_src2_mux[31])&&(!result_sum[31]));
    
	always @(*) begin
        if(rst==1'b1)begin
            //...
        end else begin
            //...
            case (alu_control)
                `ADD_OP,`ADDI_OP,`SUB_OP:begin
                    alu_result = result_sum;
                    if(ov_sum==1'b1) begin
                        wreg_o=1'b0;//产生溢出，不保存结果
                    end
                end
                `ADDU_OP,`ADDIU_OP,`SUBU_OP:begin
                    alu_result = result_sum;
                end
                default:begin
                    alu_result = 32'b0;
                end 
            endcase
        end
    end

```



### 延迟槽

分支跳转指令(JAL、JR、BEQ、BNE)

+ 将跳转指令的后一条指令地址暂时保存至延迟槽

  由于跳转指令在译码阶段才会返回将要跳转的目标地址，此时下一条指令已经取出，将其放入延迟槽中。即我们所谓的，五级流水中跳转指令的下一条指令一定会执行。这里，编译器经常在跳转指令后加一条空指令，以保证程序的正确执行。



id.v

```verilog
`timescale 1ns / 1ps
`include "defines.v"
module id(
	//...
    //延迟槽begin
    input wire is_in_delayslot_i,
    output reg is_in_delayslot_o,
    output reg next_inst_in_delayslot_o,
    //延迟槽end

    //转移指令begin
    output reg branch_flag_o,//是否发生转移
    output reg [31:0] branch_target_address_o,//转移到的目标地址
    output reg [31:0] link_addr_o,//转移地址要保存的返回地址
    //转移指令end
	//...
    );
    
    always @(*) begin
        if(rst==1'b1)begin
            //...
            //转移指令begin
            branch_flag_o<=1'b0;
            branch_target_address_o<=32'h00000000;
            link_addr_o<=32'h00000000;
            next_inst_in_delayslot_o<=1'b0;
            //转移指令end
        end else begin
            //...
            //转移指令begin
            branch_flag_o<=1'b0;
            branch_target_address_o<=32'h00000000;
            link_addr_o<=32'h00000000;
            next_inst_in_delayslot_o<=1'b0;
            //转移指令end
            case (op)
                `EXE_SPECIAL_INST:begin
                    case (op2)
                        5'b00000:begin
                            case (op3)
                                //...
                                `EXE_JR:begin
                                    aluop_o<=`JR_OP;
                                    wreg_o<=1'b0;
                                    reg1_read_o<=1'b1;
                                    reg2_read_o<=1'b0;
                                    instvalid<=1'b0;

                                    link_addr_o<=32'h00000000;
                                    branch_target_address_o<=reg1_o;
                                    branch_flag_o<=1'b1;
                                    next_inst_in_delayslot_o<=1'b1;
                                end
                                default:begin
                                    //op3 over
                                end
                            endcase
                        end
                        default:begin
                            //op2 over
                        end
                    endcase
                end
                `EXE_JAL:begin
                    aluop_o<=`JAL_OP;
                    wreg_o<=1'b1;
                    reg1_read_o<=1'b0;
                    reg2_read_o<=1'b0;
                    instvalid<=1'b0;

                    wd_o<=5'b11111;//第31号通用寄存器
                    link_addr_o<=pc_plus_8;
                    //分支对应延迟槽指令之后的指令的PC值保存至第31号通用寄存器中

                    branch_target_address_o<={pc_plus_4[31:28],inst_i[25:0],2'b00};
                    //分支指令对应的延迟槽指令的PC的最高4位与立即数instr_index左移2位后的值拼接得到
                    branch_flag_o<=1'b1;
                    next_inst_in_delayslot_o<=1'b1;
                end
                `EXE_BEQ:begin
                    aluop_o<=`BEQ_OP;
                    wreg_o<=1'b0;
                    reg1_read_o<=1'b1;
                    reg2_read_o<=1'b1;
                    instvalid<=1'b0;

                    if(reg1_o==reg2_o) begin
                        branch_target_address_o<=pc_plus_4+imm_sll2_signedext;
                        //由立即数offset左移2位并进行有符号扩展的值加上该分支指令对应的延迟槽指令的PC
                        branch_flag_o<=1'b1;
                        next_inst_in_delayslot_o<=1'b1;
                    end
                end
                `EXE_BNE:begin
                    aluop_o<=`BNE_OP;
                    wreg_o<=1'b0;
                    reg1_read_o<=1'b1;
                    reg2_read_o<=1'b1;
                    instvalid<=1'b0;

                    if(reg1_o!=reg2_o) begin
                        branch_target_address_o<=pc_plus_4+imm_sll2_signedext;
                        //由立即数offset左移2位并进行有符号扩展的值加上该分支指令对应的延迟槽指令的PC
                        branch_flag_o<=1'b1;
                        next_inst_in_delayslot_o<=1'b1;
                    end
                end
                default:begin
                    //op over
                end
            endcase
        end
    end
    
    //关于延迟槽begin
    always @(*) begin
        if(rst==1'b1)begin
            is_in_delayslot_o<=1'b0;
        end else begin
            is_in_delayslot_o<=is_in_delayslot_i;
            //表示当前译码阶段指令是否是延迟槽指令
        end
    end
    //关于延迟槽end
```



### 数据相关

HiLo相关寄存器：修改ex阶段，主要是关于访存、回写阶段的数据相关。

+ 由于Hilo模块只能写入，不能请求读出，HiLo相关读操作均在ex阶段设置的中间变量中执行。所以我们的访存、写回数据相关的判断均在执行阶段进行，且只需要判断写信号即可。

32个通用寄存器：执行、访存、回写三个阶段均涉及到有关32个通用寄存器的读写信号和数据的传递，可能存在读写冲突问题；其中，regfile模块本身会在译码阶段将信号传递读信号，所以此时译码阶段同回写阶段的读写冲突放在regfile中判断。

1. 修改id阶段，主要是关于执行、访存阶段的数据相关；
2. 修改regfile模块，主要是关于回写阶段的数据相关。



【数据前推】判断当前阶段和其他阶段的使用的源操作数是否位于同一寄存器中，通过数据前推的方式，即直接将上一条指令未回写的值提前赋值给当前阶段。

>解决32个通用寄存器的数据相关问题

id.v

```verilog
`timescale 1ns / 1ps
`include "defines.v"
module id(
	//...
    //解决数据相关问题：数据前推end
    //处于执行阶段的指令的运算结果
    input wire ex_wreg_i,//指令是否要写目的寄存器
    input wire [31:0] ex_wdata_i,
    input wire [4:0] ex_wd_i,
    input wire [5:0] ex_aluop_i,
    //处于访存阶段的指令的运算结果
    input wire mem_wreg_i,//指令是否要写目的寄存器
    input wire [31:0] mem_wdata_i,
    input wire [4:0] mem_wd_i,
    //解决数据相关问题：数据前推end

    //延迟槽begin
    input wire is_in_delayslot_i,
    output reg is_in_delayslot_o,
    output reg next_inst_in_delayslot_o,
    //延迟槽end

    //转移指令begin
    output reg branch_flag_o,//是否发生转移
    output reg [31:0] branch_target_address_o,//转移到的目标地址
    output reg [31:0] link_addr_o,//转移地址要保存的返回地址
    //转移指令end
	//...
    );
    
	//关于寄存器的数据读入begin
    always @(*) begin
        stallreq_for_reg1_loadrelate<=1'b0;
        if(rst==1'b1)begin
            reg1_o<=32'h00000000;
        //解决load相关问题
        end else if(pre_inst_is_load==1'b1&&ex_wd_i==reg1_addr_o&&reg1_read_o==1'b1) begin
            stallreq_for_reg1_loadrelate<=1'b1;
        //解决执行阶段数据相关问题   
        end else if((reg1_read_o==1'b1)&&(ex_wreg_i==1'b1)&&(ex_wd_i==reg1_addr_o)) begin
            reg1_o<=ex_wdata_i;
        //解决访存阶段数据相关问题
        end else if((reg1_read_o==1'b1)&&(mem_wreg_i==1'b1)&&(mem_wd_i==reg1_addr_o)) begin
            reg1_o<=mem_wdata_i;
        end else if(reg1_read_o==1'b1)begin
            reg1_o<=reg1_data_i;
        end else if(reg1_read_o==1'b0)begin
            reg1_o<=imm;
        end else begin
            reg1_o<=32'h00000000;
        end
    end

    always @(*) begin
        stallreq_for_reg2_loadrelate<=1'b0;
        if(rst==1'b1)begin
            reg2_o<=32'h00000000;
        //解决load相关问题
        end else if(pre_inst_is_load==1'b1&&ex_wd_i==reg2_addr_o&&reg2_read_o==1'b1) begin
            stallreq_for_reg2_loadrelate<=1'b1;
        //解决执行阶段数据相关问题   
        end else if((reg2_read_o==1'b1)&&(ex_wreg_i==1'b1)&&(ex_wd_i==reg2_addr_o)) begin
            reg2_o<=ex_wdata_i;
        //解决访存阶段数据相关问题
        end else if((reg2_read_o==1'b1)&&(mem_wreg_i==1'b1)&&(mem_wd_i==reg2_addr_o)) begin
            reg2_o<=mem_wdata_i;
        end else if(reg2_read_o==1'b1)begin
            reg2_o<=reg2_data_i;
        end else if(reg2_read_o==1'b0)begin
            reg2_o<=imm;
        end else begin
            reg2_o<=32'h00000000;
        end
    end
    //关于寄存器的数据读入end  
```

regfile.v

```verilog
	always @(*) begin
        if(rst==1)begin
            rdata1<=32'h0;
        end else if((raddr1==5'b0)&&(re1==1'b1))begin
            rdata1<=32'h0;
        end else if((raddr1==waddr)&&(we==1'b1)&&(re1==1'b1))begin//解决数据相关问题：数据前推
            rdata1<=wdata;
        end else if(re1==1'b1)begin
            rdata1<=regs[raddr1];
        end else begin
            rdata1<=32'h0;
        end
    end

    always @(*) begin
        if(rst==1)begin
            rdata2<=32'h0;
            //当复位信号（rst==1）有效时，读数据（rdata1和rdata2）为0
        end else if((raddr2==5'b0)&&(re2==1'b1))begin
            rdata2<=32'h0;
            //当复位信号（rst==0）无效时,当读地址为0，读数据为0
        end else if((raddr2==waddr)&&(we==1'b1)&&(re2==1'b1))begin//解决数据相关问题：数据前推
            rdata2<=wdata;
            //当读写地址相等，且读写使能都有效的时候，读数据为写数据【数据相关：读后写】
        end else if(re2==1'b1)begin
            rdata2<=regs[raddr2];
            //当读使能有效时，读数据为寄存器堆中存储数据
        end else begin
            rdata2<=32'h0;
        end
    end
```



> 解决HI、LO寄存器的数据相关问题

alu.v

```verilog
`timescale 1ns / 1ps
`include "defines.v"
module alu(
    //...
    //HI\LO数据相关问题begin
    //回写阶段检测寄存器
    input wire wb_whilo_i,
    input wire [31:0] wb_hi_i,
    input wire [31:0] wb_lo_i,
    //访存阶段检测寄存器
    input wire mem_whilo_i,
    input wire [31:0] mem_hi_i,
    input wire [31:0] mem_lo_i,
    //HI\LO数据相关问题end
    //...
);
    
    //HI、LO
    reg [31:0] HI;
    reg [31:0] LO;
    //解决HI、LO的数据相关问题
    always @(*) begin
        if(rst==1'b1) begin
            {HI,LO}={32'h00000000,32'h00000000};
        //访存阶段的数据相关问题
        end else if(mem_whilo_i==1'b1) begin
            {HI,LO}={mem_hi_i,mem_lo_i};
        //回写阶段的数据相关问题
        end else if(wb_whilo_i==1'b1) begin
            {HI,LO}={wb_hi_i,wb_lo_i};
        end else begin
            //HILO模块给出的寄存器值
            {HI,LO}={hi_i,lo_i};
        end
    end
```



### 流水线暂停(控制相关)

加载存储指令：修改id阶段，主要是关于load相关问题。

乘除法指令、乘累加、乘累减指令：修改ex阶段。由于指令完成运算所需要的周期不止一个，其中，除法器使用加减交替法迭代计算，需要32个计算周期，乘法器使用booth二位乘算法计算，需要16个计算周期。为了等待这些多周期指令执行完毕，需要保持取指令地址的PC值不变，同时保持流水线取指、译码、执行阶段的寄存器不变，允许访存、回写阶段的指令继续进行。



【添加控制模块ctrl，通过暂停信号stall进行流水线的判停操作，主要修改pc_reg模块的pc值是否保持不变和通道模块if_id、id_ex、ex_mem、mem_wb传递的值是否保持不变】

ctrl.v文件：

```verilog
`timescale 1ns / 1ps
//流水线暂停：乘累加、乘累减、除法
module ctrl(
    input wire rst,
    input wire stallreq_from_id,
    input wire stallreq_from_ex,
    output reg [5:0] stall
);
//stall[0]取指地址PC是否保持不变
//stall[1]取指阶段是否暂停
//stall[2]译码阶段是否暂停
//stall[3]执行阶段是否暂停
//stall[4]访存阶段是否暂停
//stall[5]回写阶段是否暂停
    always @(*) begin
        if(rst==1'b1) begin
            stall<=6'b000000;
        end else if(stallreq_from_id==1'b1) begin
            stall<=6'b000111;
        end else if(stallreq_from_ex==1'b1) begin
            stall<=6'b001111;
        end else begin
            stall<=6'b000000;
        end
    end
endmodule
```

其他部分：

1. 顶层模块例化
2. 管道模块stall暂停判断
3. id模块stallreq_from_id判断加载存储指令中的load相关问题；ex模块stallreq_from_ex判断乘除法指令、乘累加、乘累减指令等长周期指令的暂停问题

### hilo寄存器hilo.v

Hilo模块本质上只能写入，不能请求读出

HiLo相关读操作均在ex阶段设置的中间变量中执行



1.基础指令：mfhi、mflo、mthi、mtlo

2.乘除法保存结果

```verilog
`timescale 1ns / 1ps
module hilo_reg(
    input wire clk,
    input wire rst,
    
    //写端口
    input wire we,
    input wire [31:0] hi_i,
    input wire [31:0] lo_i,

    //读端口
    output reg [31:0] hi_o,
    output reg [31:0] lo_o

);

    always @(posedge clk) begin
        if(rst==1'b1) begin
            hi_o<=32'h00000000;
            lo_o<=32'h00000000;
        end else if((we==1'b1)) begin
            hi_o<=hi_i;
            lo_o<=lo_i;
        end
    end
endmodule
```

ex模块：添加hilo寄存器控制部分

```verilog
	always @(*) begin
        if(rst==1'b1)begin
            alu_result=32'h00000000;
            wd_o=5'b00000;
            wreg_o=1'b0;
            whilo_o=1'b0;
            hi_o=32'h00000000;
            lo_o=32'h00000000;
        end else begin
            wd_o=wd_i;//new:目的寄存器地址[信号传递]
            wreg_o=wreg_i;//new:是否要写入目的寄存器[信号传递]
            alu_result = 32'b0;
            whilo_o=1'b0;
            hi_o=32'h00000000;
            lo_o=32'h00000000;
            case (alu_control)
                //...
                //mfhi、mflo
                `MFHI_OP:begin
                    alu_result = HI;
                end
                `MFLO_OP:begin
                    alu_result = LO;
                end 
                //mthi、mtlo
                `MTHI_OP:begin
                    whilo_o=1'b1;
                    hi_o=alu_src1;
                    lo_o=LO;//写HI寄存器，LO寄存器保持不变
                end
                `MTLO_OP:begin
                    whilo_o=1'b1;
                    hi_o=HI;
                    lo_o=alu_src1;//写LO寄存器，HI寄存器保持不变
                end
                //mul
                `MUL_OP:begin
                    alu_result=mul_result_i[31:0];
                end
                `MULT_OP,`MULTU_OP:begin
                    whilo_o=1'b1;
                    hi_o=mul_result_i[63:32];
                    lo_o=mul_result_i[31:0];
                end
                `DIV_OP,`DIVU_OP:begin
                    whilo_o=1'b1;
                    hi_o=div_result_i[63:32];
                    lo_o=div_result_i[31:0];
                end
                default:begin
                    alu_result = 32'b0;
                    whilo_o=1'b0;
                    hi_o=32'h00000000;
                    lo_o=32'h00000000;
                end 
            endcase
        end
    end
```



### 除法器div.v

> 基础除法【writer:from wrc】
>
> 直接使用Verilog环境自带的除法(/)和取模运算(%)计算除法的商和余数

```verilog
	//除法运算
	reg[31:0] div1;//第一个乘数
	reg[31:0] div2;//第二个乘数
	reg[31:0] divs;//原码相除结果
	reg[31:0] divm;//原码相除余数
	reg[63:0] divres;//存放最终结果
	always @(*) begin
		//赋值：
		if(alu_control==`DIV_OP)begin
			if(alu_src1[31]==1'b1)begin
				div1=~alu_src1+1;//负数则取反加一
			end else begin
				div1=alu_src1;//其他情况正常赋值
			end
			if(alu_src2[31]==1'b1)begin
				div2=~alu_src2+1;//负数则取反加一
			end else begin
				div2=alu_src2;//其他情况正常赋值
			end
		end else if(alu_control==`DIVU_OP) begin
			div1=alu_src1;//无符号数乘法直接赋值
			div2=alu_src2;
		end else begin
			div1=`ZeroWord;
			div2=`ZeroWord;
		end
		
		//除法运算：
		if(rst==1'b1)begin
			divs<=0;
			divm<=0;
			divres<=0;
		end else if(alu_control==`DIV_OP) begin
			divs<=div1/div2;
			divm<=div1%div2;
			if(alu_src1[31]^alu_src2[31]==1'b1) begin
				//有符号乘法因子异号，结果为负数，应为补码
				divres<={~divm+1,~divs+1};
			end else begin
				divres<={divm,divs};
			end
		end else if(alu_control==`DIVU_OP) begin
			divs<=div1/div2;
			divm<=div1%div2;
			divres<={divm,divs};//无符号乘法直接赋值
		end
	end
```

***

迭代法

+ 试商法

+ 加减交替法、不恢复余数法
+ 恢复余数法

> 试商法
>
> 通过状态机的转换，模拟笔算除法的过程

```verilog
`timescale 1ns / 1ps
//试商法、迭代法、加减交替法、不恢复余数法
`define DivFree 2'b00
`define DivByZero 2'b01
`define DivOn 2'b10
`define DivEnd 2'b11

module div(
    input wire clk,
    input wire rst,

    input wire signed_div_i,
    input wire [31:0] opdata1_i,
    input wire [31:0] opdata2_i,
    input wire start_i,
    input wire annul_i,
    
    output reg [63:0] result_o,
    output reg ready_o
);

    wire [32:0] div_temp;
    reg [5:0] cnt;//记录试商法进行了几轮，当等于32时，表示试商法结束
    reg [64:0] dividend;
    reg [1:0] state;
    reg [31:0] divisor;
    reg [31:0] temp_op1;
    reg [31:0] temp_op2;

    //每次迭代时被除数减除数，对结果进行分析
    assign div_temp={1'b0,dividend[63:32]} - {1'b0,divisor};

    always @(posedge clk) begin
        if(rst==1'b1) begin
            state <= `DivFree;
            ready_o <=1'b0;
            result_o <={32'h00000000,32'h00000000};
        end else begin
            case (state)
                `DivFree:begin//除法的准备阶段，初始化
                    if(start_i==1'b1 && annul_i==1'b0) begin
                        if(opdata2_i==32'h00000000) begin
                            state<=`DivByZero;//除数为0
                        end else begin
                            state<=`DivOn;
                            cnt<=6'b000000;
                            if(signed_div_i==1'b1 && opdata1_i[31]==1'b1) begin
                                temp_op1=~opdata1_i+1;
                            end else begin
                                temp_op1=opdata1_i;
                            end
                            if(signed_div_i==1'b1 && opdata2_i[31]==1'b1) begin
                                temp_op2=~opdata2_i+1;
                            end else begin
                                temp_op2=opdata2_i;
                            end
                            dividend<={32'h00000000,32'h00000000};
                            dividend[32:1]<=temp_op1;
                            divisor<=temp_op2;
                        end
                        
                    end else begin
                        ready_o<=1'b0;
                        result_o <={32'h00000000,32'h00000000};
                    end
                end
                `DivByZero:begin
                    dividend<={32'h00000000,32'h00000000};
                    state<=`DivEnd;
                end
                `DivOn:begin
                    if(annul_i==1'b0) begin
                        if(cnt!=6'b100000) begin
                            if(div_temp[32]==1'b1) begin//结果为负，商补位为0
                                dividend<={dividend[63:0],1'b0};
                            end else begin//结果为正，商补位为1
                                dividend<={div_temp[31:0],dividend[31:0],1'b1};
                            end
                            cnt<=cnt+1;
                        end else begin//试商法结束后对最终结果进行修正
                            if((signed_div_i==1'b1)&&((opdata1_i[31]^opdata2_i[31])==1'b1)) begin
                                dividend[31:0]<=(~dividend[31:0]+1);
                            end//低32位求补码：商
                            if((signed_div_i==1'b1)&&((opdata1_i[31]^dividend[64])==1'b1)) begin
                                dividend[64:33]<=(~dividend[64:33]+1);
                            end//高32位求补码：余数
                            state<=`DivEnd;
                            cnt<=6'b000000;
                        end
                    end else begin
                        state<=`DivFree;
                    end
                end
                `DivEnd:begin
                    result_o<={dividend[64:33],dividend[31:0]};
                    ready_o<=1'b1;
                    if(start_i==1'b0) begin
                        state<=`DivFree;
                        result_o<={32'h00000000,32'h00000000};
                        ready_o<=1'b0;
                    end
                end
                default: begin
                    //do nothing
                end
            endcase
        end
    end
endmodule
```

ex模块：添加除法器控制部分

```verilog
//除法运算
    reg stallreq_for_div;
    always @(*) begin
        if(rst==1'b1)begin
            stallreq_for_div<=1'b0;
            div_opdata1_o<=32'h00000000;
            div_opdata2_o<=32'h00000000;
            div_start_o<=1'b0;
            signed_div_o<=1'b0;
        end else begin
            stallreq_for_div<=1'b0;
            div_opdata1_o<=32'h00000000;
            div_opdata2_o<=32'h00000000;
            div_start_o<=1'b0;
            signed_div_o<=1'b0;
            case (alu_control)
                `DIV_OP:begin//有符号除法
                    if(div_ready_i==1'b0) begin
                        div_opdata1_o<=alu_src1;
                        div_opdata2_o<=alu_src2;
                        div_start_o<=1'b1;
                        signed_div_o<=1'b1;
                        stallreq_for_div<=1'b1;
                    end else if(div_ready_i==1'b1) begin
                        div_opdata1_o<=alu_src1;
                        div_opdata2_o<=alu_src2;
                        div_start_o<=1'b0;
                        signed_div_o<=1'b1;
                        stallreq_for_div<=1'b0;
                    end else begin
                        stallreq_for_div<=1'b0;
                        div_opdata1_o<=32'h00000000;
                        div_opdata2_o<=32'h00000000;
                        div_start_o<=1'b0;
                        signed_div_o<=1'b0;
                    end
                end
                `DIVU_OP:begin
                    if(div_ready_i==1'b0) begin
                        div_opdata1_o<=alu_src1;
                        div_opdata2_o<=alu_src2;
                        div_start_o<=1'b1;
                        signed_div_o<=1'b0;
                        stallreq_for_div<=1'b1;
                    end else if(div_ready_i==1'b1) begin
                        div_opdata1_o<=alu_src1;
                        div_opdata2_o<=alu_src2;
                        div_start_o<=1'b0;
                        signed_div_o<=1'b0;
                        stallreq_for_div<=1'b0;
                    end else begin
                        stallreq_for_div<=1'b0;
                        div_opdata1_o<=32'h00000000;
                        div_opdata2_o<=32'h00000000;
                        div_start_o<=1'b0;
                        signed_div_o<=1'b0;
                    end
                end
                default:begin
                    //do nothing
                end
            endcase
        end
    end
```

***

其他代码：

<https://www.cnblogs.com/moranhuishou0315/p/11344725.html>

<https://blog.csdn.net/qq_38600065/article/details/104665427>



### 乘法器mul.v

> 基础乘法
>
> 直接使用Verilog环境自带的乘(×)计算乘法的高32位和低32位

这里给出两种Verilog代码，大体思路相似，逻辑电路实现上有所不同。

```verilog
	//乘法运算：
	reg[31:0] mult1;//第一个乘数
	reg[31:0] mult2;//第二个乘数
	reg[63:0] muls;//原码相乘结果
	reg[63:0] mulres;//存放最终结果
	always @(*) begin
		//赋值：
		if(alu_control==`MULT_OP)begin
			if(alu_src1[31]==1'b1)begin
				mult1=~alu_src1+1;//负数则取反加一
			end else begin
				mult1=alu_src1;//其他情况正常赋值
			end
			if(alu_src2[31]==1'b1)begin
				mult2=~alu_src2+1;//负数则取反加一
			end else begin
				mult2=alu_src2;//其他情况正常赋值
			end
		end else if(alu_control==`MULTU_OP) begin
			mult1=alu_src1;//无符号数乘法直接赋值
			mult2=alu_src2;
		end else begin
			mult1=`ZeroWord;
			mult2=`ZeroWord;
		end
		
		//乘法运算：
		if(rst==1'b1)begin
			muls<=0;
			mulres<=0;
		end else if(alu_control==`MULT_OP) begin
			muls=mult1*mult2;
			if(alu_src1[31]^alu_src2[31]==1'b1) begin
				//有符号乘法因子异号，结果为负数，应为补码
				mulres<=~muls+1;
			end else begin
				mulres<=muls;
			end
		end else if(alu_control==`MULTU_OP) begin
			muls=mult1*mult2;
			mulres<=muls;//无符号乘法直接赋值
		end
	end
```



```verilog
	//乘法运算
    wire [31:0] opdata1_mult;//第一个乘数
    wire [31:0] opdata2_mult;//第二个乘数
    wire [63:0] hilo_temp;//结果
    //有符号数乘法，乘数为负数时取反加1；无符号数乘法，乘数保持不变
    assign opdata1_mult=(((alu_control==`MUL_OP)||(alu_control==`MULT_OP))&&(alu_src1[31]==1'b1)) ? (~alu_src1+1) : alu_src1;
    assign opdata2_mult=(((alu_control==`MUL_OP)||(alu_control==`MULT_OP))&&(alu_src2[31]==1'b1)) ? (~alu_src2+1) : alu_src2;
    //这里采用直接乘的办法【后续可修改为booth二位乘或华莱士树】
    assign hilo_temp=opdata1_mult*opdata2_mult;
    //对临时乘法结果进行修正：
    //1.有符号乘法mult、mul：乘数异号，hilo_temp求补码作为最终结果；乘数同号，直接赋值
    //2.无符号乘法multu：直接赋值
    reg [63:0] mulres;
    always @(*) begin
        if(rst==1'b1) begin
            mulres<={32'h00000000,32'h00000000};
        end else if((alu_control==`MUL_OP)||(alu_control==`MULT_OP)) begin
            if(alu_src1[31]^alu_src2[31]==1'b1) begin//有符号乘法mult、mul，乘数异号，求反加1
                mulres<=~hilo_temp+1;
            end else begin//有符号乘法mult、mul，乘数同号，直接赋值
                mulres<=hilo_temp;
            end
        end else begin//无符号乘法multu：直接赋值
            mulres<=hilo_temp;
        end
    end
```

***

+ 移位乘法器
+ booth一位乘
+ booth二位乘
+ 华莱士树（基础部分积为booth二位乘）

> booth一位乘【writer:from yyz】
>
> 每一步选择第二个乘数的两位进行判断，得到对第一个乘数的相关运算（+/-/0）

$Y=-y_{7}*2^7+y_{6}*2^6+y_{5}*2^5+y_{4}*2^4+y_{3}*2^3+y_{2}*2^2+y_{1}*2^1+y_{0}*2^0$

$Y=-y_{7}*2^7+(2-1)y_{6}*2^6+(2-1)y_{5}*2^5+(2-1)y_{4}*2^4+(2-1)y_{3}*2^3+(2-1)y_{2}*2^2+(2-1)y_{1}*2^1+(2-1)y_{0}*2^0$

$Y=-y_{7}*2^7+(y_{6}*2^7-y_{6}*2^6)+(y_{5}*2^6-y_{5}*2^5)+(y_{4}*2^5-y_{4}*2^4)+......+(y_{0}*2^1-y_{0}*2^0)$

$Y=(y_{6}-y_{7})*2^7+(y_{5}-y_{6})*2^6+(y_{4}-y_{5})*2^5+......+(y_{-1}-y_{0})*2^0$

其中，$y_{-1}=0$

![2021-07-19-mul8.png](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-07-19-mul8.png)

```verilog
`timescale 1ns / 1ps
`include "defines.v"

module mult(
           input wire clk,
           input wire rst,

           input wire signed_mult_i,// 是否为有符号乘法,为1是表示有符号
           input wire[31:0] opdata1_i,// 乘数1(被乘数)
           input wire[31:0] opdata2_i,// 乘数2(乘数)
           input wire start_i,// 是否开始乘法运算

           output reg[63:0] result_o,
           output reg ready_o
       );

reg[5:0] count;// 当count等于32时,表示迭代过程结束,需要做最后一个加法或者减法
reg[1:0] state;// 状态 MultFree=00,MultOn=01,MultEnd=10
reg[33:0] reg_A;
reg[33:0] reg_X;// X存放被乘数的补码(含两位符号位)
reg[33:0] reg_Q;// 存放乘数的补码(含最高1位符号位和最末1位附加位)

reg[33:0] neg_data1;// 被乘数的相反数的补码

always @(posedge clk) begin
    if(rst==`RstEnable) begin
        state <= `MultFree;
        ready_o <= `MultResultNotReady;
        result_o <= {`ZeroWord,`ZeroWord};
    end
    else begin
        case(state)
            /*
                MultFree状态
            */
            `MultFree: begin
                if(start_i==`MultStart) begin
                    state <= `MultOn;
                    count <= 6'b000000;
                    reg_A <= 34'b0;// 乘法运算前A寄存器被清零,作为初始部分积
                    if (signed_mult_i!=1'b1) begin
                        reg_X <= { {2{1'b0} },opdata1_i};
                        reg_Q <= {1'b0,opdata2_i,1'b0};
                    end
                    else begin
                        reg_X <= { {2{opdata1_i[31]} },opdata1_i};
                        reg_Q <= {opdata2_i[31],opdata2_i,1'b0};
                    end
                    neg_data1 <= { {2{~(opdata1_i[31])} },(~opdata1_i)+1};
                end
                else begin
                    ready_o <= `MultResultNotReady;
                    result_o <= {`ZeroWord,`ZeroWord};
                end
            end
            /*
                MultOn状态
            */
            `MultOn: begin
                if(count!=6'b100000) begin
                    case(reg_Q[1:0])
                        2'b10: begin
                            reg_A = reg_A + neg_data1;
                            // flag <= 1'b1;
                        end
                        2'b01: begin
                            reg_A = reg_A + reg_X;
                        end
                    endcase
                    reg_Q = {reg_A[0],reg_Q[33:1]};
                    reg_A = {reg_A[33],reg_A[33:1]};//算术右移一位
                    count <= count + 1;
                end
                else begin
                    // 最后还要再判断一次Qn和Qn+1
                    case(reg_Q[1:0])
                        2'b10: begin
                            reg_A <= reg_A + neg_data1;
                        end
                        2'b01: begin
                            reg_A <= reg_A + reg_X;
                        end
                    endcase
                    // end
                    state <= `MultEnd;
                    count <= 6'b000000;
                end
            end
            /*
                MultEnd(乘法运算结束)
                result_o的宽度64,
            */
            `MultEnd: begin
                result_o <= {reg_A[31:0],reg_Q[33:2]};
                ready_o <= `MultResultReady;//乘法结束
                if(start_i==`MultStop) begin
                    state <= `MultFree;
                    ready_o <= `MultResultNotReady;
                    result_o <= {`ZeroWord,`ZeroWord};
                    reg_A <= 34'b0;
                    reg_X <= 34'b0;
                    reg_Q <= 34'b0;
                    neg_data1 = 34'b0;
                end
            end
        endcase
    end
end

endmodule
```



> booth二位乘【writer:from djx】
>
> 每一步选择第二个乘数的三位进行判断，得到对第一个乘数的相关运算（+/-/0/+2/-2）
>
> 对比前面的booth一位乘，时钟周期减半

$Y=-y_{7}*2^7+y_{6}*2^6+y_{5}*2^5+y_{4}*2^4+y_{3}*2^3+y_{2}*2^2+y_{1}*2^1+y_{0}*2^0$

$Y=-2*y_{7}*2^6+y_{6}*2^6+(y_{5}*2^6-2*y_{5}*2^4)+y_{4}*2^4+(y_{3}*2^4-2*y_{3}*2^2)+y_{2}*2^2+(y_{1}*2^2-2*y_{1}*2^0)+y_{0}*2^0+y_{-1}*2^0$

$Y=(y_{5}+y_{6}-2*y_{7})*2^6+(y_{3}+y_{4}-2*y_{5})*2^4+(y_{1}+y_{2}-2*y_{3})*2^2+(y_{-1}+y_{0}-2*y_{1})*2^0$

![2021-07-19-mul10.png](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-07-19-mul10.png)

```verilog
`timescale 1ns / 1ps
`define MulFree 2'b00
`define MulByZero 2'b01
`define MulOn 2'b10
`define MulEnd 2'b11
//基本32位的booth二位乘已完成
//按照累乘/除法运算的思路修改CPU【本质上只要连接好接口即可】
module mul(
    input wire clk,
    input wire rst,
    
    input wire signed_mul_i,
    input wire [31:0] mul1,mul2,
    input wire start_i,
    input wire annul_i,
    
    output reg [63:0] res,
    output reg ready_o
);

    reg [4:0] cnt;//运算节拍，相当于状态机了，8位的话每次运算有4个拍
    //由于传值默认就是补码，所以只需要再计算“负补码”即可
    reg [1:0] state;

    wire [31:0] bmul1;
    assign bmul1 = (~mul1 + 1'b1);

    parameter   ph_0_1      =   5'b00000,
                ph_2_3      =   5'b00001,
                ph_4_5      =   5'b00010,
                ph_6_7      =   5'b00011,
                ph_7_8      =   5'b00100,
                ph_9_10     =   5'b00101,
                ph_11_12    =   5'b00110,
                ph_13_14    =   5'b00111,
                ph_15_16    =   5'b01000,
                ph_17_18    =   5'b01001,
                ph_19_20    =   5'b01010,
                ph_21_22    =   5'b01011,
                ph_23_24    =   5'b01100,
                ph_25_26    =   5'b01101,
                ph_27_28    =   5'b01110,
                ph_29_30    =   5'b01111,
                ph_31_32    =   5'b10000;
    //y(i-1),y(i),y(i+1)三个数的判断寄存器，由于有多种情况，也可以看成状态机（也可以改写成状态机形式，大家自己试试吧）
    reg [2:0] temp;

    //部分积
    reg [63:0] A;
	//每个节拍下把相应位置的数据传给temp寄存器
    always @ (posedge clk) begin
        case(cnt)
            ph_0_1  : temp <= {mul2[1:0],1'b0};
            ph_2_3  : temp <= mul2[3:1];
            ph_4_5  : temp <= mul2[5:3];
            ph_6_7  : temp <= mul2[7:5];
            ph_7_8  : temp <= mul2[9:7];
            ph_9_10  : temp <= mul2[11:9];
            ph_11_12  : temp <= mul2[13:11];
            ph_13_14  : temp <= mul2[15:13];
            ph_15_16  : temp <= mul2[17:15];
            ph_17_18  : temp <= mul2[19:17];
            ph_19_20  : temp <= mul2[21:19];
            ph_21_22  : temp <= mul2[23:21];
            ph_23_24  : temp <= mul2[25:23];
            ph_25_26  : temp <= mul2[27:25];
            ph_27_28  : temp <= mul2[29:27];
            ph_29_30  : temp <= mul2[31:29];
            ph_31_32  : temp <= (signed_mul_i==1'b1) ? {3{mul2[31]}} : 3'b000;
            default : temp <= 0;
        endcase
    end
	

    always @(posedge clk) begin
        if(rst==1'b1) begin
            state <= `MulFree;
            ready_o <=1'b0;
            res <={32'h00000000,32'h00000000};
        end else begin
            case (state)
                `MulFree:begin//除法的准备阶段，初始化
                    if(start_i==1'b1 && annul_i==1'b0) begin
                        if(mul2==32'h00000000||mul1==32'h00000000) begin
                            state<=`MulByZero;//某一乘数为0
                        end else begin
                            state<=`MulOn;
                            cnt<=5'b00000;
                            A<=32'h00000000;
                            //dividend<={32'h00000000,32'h00000000};
                            //dividend[32:1]<=temp_op1;
                            //divisor<=temp_op2;
                        end
                        
                    end else begin
                        ready_o<=1'b0;
                        res <={32'h00000000,32'h00000000};
                    end
                end
                `MulByZero:begin
                    A<={32'h00000000,32'h00000000};
                    state<=`MulEnd;
                end
                `MulOn:begin
                    if(annul_i==1'b0) begin
                        if(cnt!=5'b10010) begin
                            if(signed_mul_i==1'b1) begin
                                case (temp)
                                    3'b000,3'b111 :   begin//这些是从高位到低位的判断，别看反了噢
                                        A <= A + 0;
                                    end
                                    3'b001,3'b010 : begin//加法操作使用补码即可，倍数利用左移解决 //加mul1的补码
                                        A <= A + ({ {32{mul1[31]} },mul1} << 2*(cnt-1));
                                    end
                                    3'b011 : begin//加2×mul1的补码
                                        A <= A + ({ {32{mul1[31]} },mul1} << 2*(cnt-1) + 1);
                                    end
                                    3'b100: begin//减法操作利用“负补码”改成加法操作，倍数利用左移解决 //减2×mul1的补码
                                        A <= A + ({ {32{bmul1[31]} },bmul1} << 2*(cnt-1) + 1);
                                    end
                                    3'b101,3'b110 : begin//减mul1的补码
                                        A <= A + ({ {32{bmul1[31]} },bmul1} << 2*(cnt-1));
                                    end
                                    default: A <= 0;
                                endcase
                            end else begin
                                case (temp)
                                    3'b000,3'b111 :   begin//这些是从高位到低位的判断，别看反了噢
                                        A <= A + 0;
                                    end
                                    3'b001,3'b010 : begin//加法操作使用补码即可，倍数利用左移解决 //加mul1的补码
                                        A <= A + ({ {32{mul1[31]} },mul1} << 2*(cnt-1));
                                    end
                                    3'b011 : begin//加2×mul1的补码
                                        A <= A + ({ {32{mul1[31]} },mul1} << 2*(cnt-1) + 1);
                                    end
                                    3'b100: begin//减法操作利用“负补码”改成加法操作，倍数利用左移解决 //减2×mul1的补码
                                        A <= A + ({ {32{bmul1[31]} },bmul1} << 2*(cnt-1) + 1);
                                    end
                                    3'b101,3'b110 : begin//减mul1的补码
                                        A <= A + ({ {32{bmul1[31]} },bmul1} << 2*(cnt-1));
                                    end
                                    default: A <= 0;
                                endcase
                                if(cnt==5'b10001) begin
                                    A[63:32] <= A[63:32] + ((mul2[31]==1'b1) ? mul1 : 0);
                                end
                                
                            end
                            cnt<=cnt+1;
                        end else begin//对最终乘法结果进行修正
                            A<=A;
                            state<=`MulEnd;
                            cnt<=6'b000000;
                        end
                    end else begin
                        state<=`MulFree;
                    end
                end
                `MulEnd:begin
                    res<=A;
                    ready_o<=1'b1;
                    if(start_i==1'b0) begin
                        state<=`MulFree;
                        res<={32'h00000000,32'h00000000};
                        ready_o<=1'b0;
                    end
                end
                default: begin
                    //do nothing
                end
            endcase
        end
    end
endmodule
```

ex模块：添加乘法器控制部分

```verilog
	//乘法运算
	reg stallreq_for_mul;
    always @(*) begin
        if(rst==1'b1)begin
            stallreq_for_mul<=1'b0;
            mul_opdata1_o<=32'h00000000;
            mul_opdata2_o<=32'h00000000;
            mul_start_o<=1'b0;
            signed_mul_o<=1'b0;
        end else begin
            stallreq_for_mul<=1'b0;
            mul_opdata1_o<=32'h00000000;
            mul_opdata2_o<=32'h00000000;
            mul_start_o<=1'b0;
            signed_mul_o<=1'b0;
            case (alu_control)
                `MUL_OP,`MULT_OP:begin//有符号乘
                    if(mul_ready_i==1'b0) begin
                        mul_opdata1_o<=alu_src1;
                        mul_opdata2_o<=alu_src2;
                        mul_start_o<=1'b1;
                        signed_mul_o<=1'b1;
                        stallreq_for_mul<=1'b1;
                    end else if(mul_ready_i==1'b1) begin
                        mul_opdata1_o<=alu_src1;
                        mul_opdata2_o<=alu_src2;
                        mul_start_o<=1'b0;
                        signed_mul_o<=1'b1;
                        stallreq_for_mul<=1'b0;
                    end else begin
                        stallreq_for_mul<=1'b0;
                        mul_opdata1_o<=32'h00000000;
                        mul_opdata2_o<=32'h00000000;
                        mul_start_o<=1'b0;
                        signed_mul_o<=1'b0;
                    end
                end
                `MULTU_OP:begin
                    if(mul_ready_i==1'b0) begin
                        mul_opdata1_o<=alu_src1;
                        mul_opdata2_o<=alu_src2;
                        mul_start_o<=1'b1;
                        signed_mul_o<=1'b0;
                        stallreq_for_mul<=1'b1;
                    end else if(mul_ready_i==1'b1) begin
                        mul_opdata1_o<=alu_src1;
                        mul_opdata2_o<=alu_src2;
                        mul_start_o<=1'b0;
                        signed_mul_o<=1'b0;
                        stallreq_for_mul<=1'b0;
                    end else begin
                        stallreq_for_mul<=1'b0;
                        mul_opdata1_o<=32'h00000000;
                        mul_opdata2_o<=32'h00000000;
                        mul_start_o<=1'b0;
                        signed_mul_o<=1'b0;
                    end
                end
                default:begin
                    //do nothing
                end
            endcase
        end
    end
```



> 华莱士树【writer:from chs】
>
> 部分积用**booth二位乘**的结果表示，分步计算出16个部分积，通过**加法树**将部分积和进位分别相加

【这个代码直接复制的chs学长的，有问题可以直接去问他，注释因为编码问题裂开了，大家自行理解】

mul.v

```verilog
`include "define.v"
module mul(
    input clk,
    input resetn,
    input flush,
    //input flush_cause,
    input request,
    input[31:0] x, 
    input[31:0] y, 
    input s, //1 sign 0 unsign
    output reg ready,
    output[63:0] z 
    );
    
    wire clearn;
    reg state;
    reg s_next;
    reg[63:0] adder1;
    reg[63:0] adder2;
    //(flush == 1'b1 && flush_cause == 1'b0)
    assign clearn = (resetn == `RstEnable || flush == 1'b1 ) ? 1'b0 : 1'b1;
    
    //状态机：第一步
    always @ (*) begin
        if (clearn == 1'b0) s_next = `MUL_IDLE;
        else begin
            case (state)
            `MUL_IDLE: begin
                if (request) s_next = `MUL_ON;
                else s_next = `MUL_IDLE;
            end
            `MUL_ON: s_next = `MUL_IDLE;
            default: s_next = `MUL_IDLE;
            endcase
        end
    end
    
    //状态机：第二步
    always @ (posedge clk) state <= s_next;
    
    wire[32:0] x_ext;
    wire[33:0] y_ext;
    wire[63:0] pp0, pp1, pp2, pp3, pp4, pp5, pp6, pp7, pp8, pp9, pp10, pp11, pp12, pp13, pp14, pp15, pp16;
    wire[63:0] pp17;
    wire[33:0] c;

    wire[63:0] s_l1_1, s_l1_2, s_l1_3, s_l1_4, s_l1_5, s_l1_6;
    wire[63:0] c_l1_1, c_l1_2, c_l1_3, c_l1_4, c_l1_5, c_l1_6;

    wire[63:0] s_l2_1, s_l2_2, s_l2_3, s_l2_4;
    wire[63:0] c_l2_1, c_l2_2, c_l2_3, c_l2_4;

    wire[63:0] s_l3_1, s_l3_2;
    wire[63:0] c_l3_1, c_l3_2;

    wire[63:0] s_l4_1, s_l4_2;
    wire[63:0] c_l4_1, c_l4_2;

    wire[63:0] s_l5_1;
    wire[63:0] c_l5_1;

    wire[63:0] s_l6_1;
    wire[63:0] c_l6_1;
    
    assign x_ext = s ? {x[31], x} : {1'b0, x};
    assign y_ext = s ? { {2{y[31]} }, y} : {2'b00, y};
    
    //booth二位乘计算17个部分积【由于有符号和无符号乘法一起计算，需要加符号位，所以为17个部分积】
    booth2 u_b0(.x(x_ext),.y({y_ext[1:0], 1'b0}), .z(pp0), .c(c[1:0]));
    booth2 u_b1(.x(x_ext), .y(y_ext[3:1]), .z(pp1), .c(c[3:2]));
    booth2 u_b2(.x(x_ext), .y(y_ext[5:3]), .z(pp2), .c(c[5:4]));
    booth2 u_b3(.x(x_ext), .y(y_ext[7:5]), .z(pp3), .c(c[7:6]));
    booth2 u_b4(.x(x_ext), .y(y_ext[9:7]), .z(pp4), .c(c[9:8]));
    booth2 u_b5(.x(x_ext), .y(y_ext[11:9]), .z(pp5), .c(c[11:10]));
    booth2 u_b6(.x(x_ext), .y(y_ext[13:11]), .z(pp6), .c(c[13:12]));
    booth2 u_b7(.x(x_ext), .y(y_ext[15:13]), .z(pp7), .c(c[15:14]));
    booth2 u_b8(.x(x_ext), .y(y_ext[17:15]), .z(pp8), .c(c[17:16]));
    booth2 u_b9(.x(x_ext), .y(y_ext[19:17]), .z(pp9), .c(c[19:18]));
    booth2 u_b10(.x(x_ext), .y(y_ext[21:19]), .z(pp10), .c(c[21:20]));
    booth2 u_b11(.x(x_ext), .y(y_ext[23:21]), .z(pp11), .c(c[23:22]));
    booth2 u_b12(.x(x_ext), .y(y_ext[25:23]), .z(pp12), .c(c[25:24]));
    booth2 u_b13(.x(x_ext), .y(y_ext[27:25]), .z(pp13), .c(c[27:26]));
    booth2 u_b14(.x(x_ext), .y(y_ext[29:27]), .z(pp14), .c(c[29:28]));
    booth2 u_b15(.x(x_ext), .y(y_ext[31:29]), .z(pp15), .c(c[31:30]));
    booth2 u_b16(.x(x_ext), .y(y_ext[33:31]), .z(pp16), .c(c[33:32]));
    
    assign pp17 = {30'b0, c};
    
    //华莱士树
    csa u_csa_l1_1(
        .x(pp0),
        .y({pp1[61:0], 2'b0}),
        .z({pp2[59:0], 4'b0}),
        .s(s_l1_1),
        .c(c_l1_1)
        );
    csa u_csa_l1_2(
        .x({pp3[57:0], 6'b0}),
        .y({pp4[55:0], 8'b0}),
        .z({pp5[53:0], 10'b0}),
        .s(s_l1_2),
        .c(c_l1_2)
        );
    csa u_csa_l1_3(
        .x({pp6[51:0], 12'b0}),
        .y({pp7[49:0], 14'b0}),
        .z({pp8[47:0], 16'b0}),
        .s(s_l1_3),
        .c(c_l1_3)
        );
    csa u_csa_l1_4(
        .x({pp9[45:0], 18'b0}),
        .y({pp10[43:0], 20'b0}),
        .z({pp11[41:0], 22'b0}),
        .s(s_l1_4),
        .c(c_l1_4)
        );
    csa u_csa_l1_5(
        .x({pp12[39:0], 24'b0}),
        .y({pp13[37:0], 26'b0}),
        .z({pp14[35:0], 28'b0}),
        .s(s_l1_5),
        .c(c_l1_5)
        );
    csa u_csa_l1_6(
        .x({pp15[33:0], 30'b0}),
        .y({pp16[31:0], 32'b0}),
        .z(pp17),
        .s(s_l1_6),
        .c(c_l1_6)
        );
    csa u_csa_l2_1(
        .x(s_l1_1),
        .y(s_l1_2),
        .z(s_l1_3),
        .s(s_l2_1),
        .c(c_l2_1)
        );
    csa u_csa_l2_2(
        .x(s_l1_4),
        .y(s_l1_5),
        .z(s_l1_6),
        .s(s_l2_2),
        .c(c_l2_2)
        );
    csa u_csa_l2_3(
        .x({c_l1_1[62:0], 1'b0}),
        .y({c_l1_2[62:0], 1'b0}),
        .z({c_l1_3[62:0], 1'b0}),
        .s(s_l2_3),
        .c(c_l2_3)
        );
    csa u_csa_l2_4(
        .x({c_l1_4[62:0], 1'b0}),
        .y({c_l1_5[62:0], 1'b0}),
        .z({c_l1_6[62:0], 1'b0}),
        .s(s_l2_4),
        .c(c_l2_4)
        );
    csa u_csa_l3_1(
        .x(s_l2_1),
        .y(s_l2_2),
        .z(s_l2_3),
        .s(s_l3_1),
        .c(c_l3_1)
        );
    csa u_csa_l3_2(
        .x(s_l2_4),
        .y({c_l2_1[62:0], 1'b0}),
        .z({c_l2_2[62:0], 1'b0}),
        .s(s_l3_2),
        .c(c_l3_2)
        );
    csa u_csa_l4_1(
        .x(s_l3_1),
        .y(s_l3_2),
        .z({c_l3_1[62:0], 1'b0}),
        .s(s_l4_1),
        .c(c_l4_1)
        );
    csa u_csa_l4_2(
        .x({c_l3_2[62:0], 1'b0}),
        .y({c_l2_3[62:0], 1'b0}),
        .z({c_l2_4[62:0], 1'b0}),
        .s(s_l4_2),
        .c(c_l4_2)
        );
    csa u_csa_l5_1(
        .x(s_l4_1),
        .y(s_l4_2),
        .z({c_l4_1[62:0], 1'b0}),
        .s(s_l5_1),
        .c(c_l5_1)
        );
    csa u_csa_l6_1(
        .x(s_l5_1),
        .y({c_l5_1[62:0], 1'b0}),
        .z({c_l4_2[62:0], 1'b0}),
        .s(s_l6_1),
        .c(c_l6_1)
        );

    //状态机：第三步
    always @ (posedge clk) begin
        case (s_next)
        `MUL_IDLE: begin
            adder1 <= {`ZeroWord, `ZeroWord};
            adder2 <= {`ZeroWord, `ZeroWord};
            ready <= 1'b0;
        end
        `MUL_ON: begin
            adder1 <= s_l6_1;
            adder2 <= {c_l6_1[62:0], 1'b0};
            ready <= 1'b1;
        end
        default: begin
            adder1 <= {`ZeroWord, `ZeroWord};
            adder2 <= {`ZeroWord, `ZeroWord};
            ready <= 1'b0;
        end
        endcase
    end
    
    fa64 u_fa64(.a(adder1),
     .b(adder2), .cin(1'b0),
      .sub(1'b0), .s(z), .cout()
    );
    
endmodule

```

booth2.v

```verilog
//booth二位乘乘法器【第二个乘数只有3位】
module booth2(
    input[32:0] x,
    input[2:0] y,
    output reg[63:0] z,
    output reg[1:0] c
    );
    wire[32:0] x_neg;
    assign x_neg = ~x;
    always @ * begin
        case(y)
            3'b011: begin
                z = { {30{x[32]} }, x, 1'b0};
                c = 2'b00;
            end
            3'b100: begin
                z = { {30{x_neg[32]} }, x_neg, 1'b0};
                c = 2'b10;
            end
            3'b001, 3'b010: begin
                z = { {31{x[32]} }, x};
                c = 2'b00;
            end
            3'b101, 3'b110: begin
                z = { {31{x_neg[32]} }, x_neg};
                c = 2'b01;
            end
            default: begin
                z = 64'b0;
                c = 2'b00;
            end
        endcase
    end
endmodule
```

csa.v

```verilog
//64位保留进位加法器
//用来计算华莱士树中的每一个节点的结果
module csa(
    input[63:0] x,
    input[63:0] y,
    input[63:0] z,
    output[63:0] s,
    output[63:0] c
    );
    genvar i;
    generate
        for (i=0; i<64; i=i+1)
        begin: 
            assign s[i] = x[i] ^ y[i] ^ z[i];
            assign c[i] = (x[i] & y[i]) | (y[i] & z[i]) | (z[i] & x[i]);
        end
    endgenerate
endmodule
```

【这里事先解释一个下面组合的加法器，同样是类似于级联加法器和加法器模块例化的结合】

使用下面三步将每一个n位加法器实现：

+ 优先计算fa模块中的g、p

+ 通过输入g、p、cin计算cla模块中的c、gout、pout

+ 通过输入cin计算fa模块中的s、cout

  

一个逻辑门的扇入和扇出是有限制的，为了减少扇入和扇出，所以我们使用分层

[什么是扇入和扇出？](https://blog.csdn.net/weixin_44648216/article/details/113851049)

fa64.v

```verilog
//64位超前进位加法器：4个16位加法器分部计算
module fa64(
    input[63:0] a,
    input[63:0] b,
    input cin,
    input sub,
    output[63:0] s,
    output[63:0] cout
    );
    wire[63:0] b_in;
    wire cin_in;
    wire[3:0] p;
    wire[3:0] g;
    wire[3:0] c;
    assign b_in = sub ? ~b : b;
    assign cin_in = sub ^ cin;
    fa16 u_fa160(.a(a[15:0]), .b(b_in[15:0]), .cin(cin_in), .gout(g[0]), .pout(p[0]), .s(s[15:0]), .cout(cout[15:0]));
    fa16 u_fa161(.a(a[31:16]), .b(b_in[31:16]), .cin(c[0]), .gout(g[1]), .pout(p[1]), .s(s[31:16]), .cout(cout[31:16]));
    fa16 u_fa162(.a(a[47:32]), .b(b_in[47:32]), .cin(c[1]), .gout(g[2]), .pout(p[2]), .s(s[47:32]), .cout(cout[47:32]));
    fa16 u_fa163(.a(a[63:48]), .b(b_in[63:48]), .cin(c[2]), .gout(g[3]), .pout(p[3]), .s(s[63:48]), .cout(cout[63:48]));
    cla u_cla(.p(p), .g(g), .cin(cin), .cout(c), .pout(), .gout());
endmodule
```

fa16.v

```verilog
//16位超前进位加法器：4个4位加法器分部计算
module fa16(
    input[15:0] a,
    input[15:0] b,
    input cin,
    output gout,
    output pout,
    output[15:0] s,
    output[15:0] cout
    );
    wire[3:0] p;
    wire[3:0] g;
    wire[3:0] c;
    fa4 u_fa40(.a(a[3:0]), .b(b[3:0]), .cin(cin), .gout(g[0]), .pout(p[0]), .s(s[3:0]), .cout(cout[3:0]));
    fa4 u_fa41(.a(a[7:4]), .b(b[7:4]), .cin(c[0]), .gout(g[1]), .pout(p[1]), .s(s[7:4]), .cout(cout[7:4]));
    fa4 u_fa42(.a(a[11:8]), .b(b[11:8]), .cin(c[1]), .gout(g[2]), .pout(p[2]), .s(s[11:8]), .cout(cout[11:8]));
    fa4 u_fa43(.a(a[15:12]), .b(b[15:12]), .cin(c[2]), .gout(g[3]), .pout(p[3]), .s(s[15:12]), .cout(cout[15:12]));
    cla u_cla(.p(p), .g(g), .cin(cin), .cout(c), .gout(gout), .pout(pout));
endmodule
```

fa4.v

```verilog
//4位超前进位加法器：4个1位全加器分部计算
module fa4(
    input[3:0] a,
    input[3:0] b,
    input cin,
    output gout,
    output pout,
    output[3:0] s,
    output[3:0] cout
    );
    wire[3:0] p;
    wire[3:0] g;
    wire[3:0] c;
    assign cout[3] = c[3];
    fa u_fa0(.a(a[0]), .b(b[0]), .cin(cin), .g(g[0]), .p(p[0]), .s(s[0]), .cout(cout[0]));
    fa u_fa1(.a(a[1]), .b(b[1]), .cin(c[0]), .g(g[1]), .p(p[1]), .s(s[1]), .cout(cout[1]));
    fa u_fa2(.a(a[2]), .b(b[2]), .cin(c[1]), .g(g[2]), .p(p[2]), .s(s[2]), .cout(cout[2]));
    fa u_fa3(.a(a[3]), .b(b[3]), .cin(c[2]), .g(g[3]), .p(p[3]), .s(s[3]), .cout());
    cla u_cla(.p(p), .g(g), .cin(cin), .cout(c), .gout(gout), .pout(pout));
endmodule
```

 fa.v

```verilog
//1位全加器
module fa(
    input a,
    input b,
    input cin,
    output g,
    output p,
    output s,
    output cout
    );
    assign s = a ^ b ^ cin;
    assign cout = (a & b) | (a & cin) | (b & cin);
    assign g = a & b;
    assign p = a | b;
endmodule
```

cla.v

```verilog
//4位级联加法器【核心计算部分】：计算超前进位g、p两个函数
module cla(
    input[3:0] p,
    input[3:0] g,
    input cin,
    output[3:0] cout,
    output gout,
    output pout
    );
    assign cout[0] = g[0] | (p[0] & cin);
    assign cout[1] = g[1] | (p[1] & g[0]) | (p[1] & p[0] & cin);
    assign cout[2] = g[2] | (p[2] & g[1]) | (p[2] & p[1] & g[0]) | (p[2] & p[1] & p[0] & cin);
    assign gout = g[3] | (p[3] & g[2]) | (p[3] & p[2] & g[1]) | (p[3] & p[2] & p[1] & g[0]);
    assign pout = p[0] & p[1] & p[2] & p[3];
    assign cout[3] = gout | (pout & cin);
endmodule
```



### cp0寄存器cp0.v

1. 协处理器访问指令：mtc0、mfc0

```verilog
`timescale 1ns / 1ps
`include "defines.v"

module cp0_reg(
    input wire clk,
    input wire rst,
    
    input wire we_i,
    input wire[4:0] waddr_i,
    input wire[4:0] raddr_i,
    input wire[`RegBus] data_i,
    
    input wire[5:0] int_i,
    
    input wire[31:0] excepttype_i,
    input wire is_in_delayslot_i,
    input wire[`RegBus] current_inst_addr_i,

    input wire [`RegBus] mem_addr_i,
    
    output reg[`RegBus] data_o,
    output reg[`RegBus] count_o,
    output reg[`RegBus] compare_o,
    output reg[`RegBus] status_o,
    output reg[`RegBus] cause_o,
    output reg[`RegBus] epc_o,
    output reg[`RegBus] config_o,
    output reg[`RegBus] prid_o,
    
    output reg timer_int_o
    
    );
    
    always @ (posedge clk) begin
        if(rst==`RstEnable) begin
            count_o<=`ZeroWord;
            compare_o<=`ZeroWord;
            status_o<=32'b00010000000000000000000000000000;
            cause_o<=`ZeroWord;
            epc_o<=`ZeroWord;
            config_o<=32'b00000000000000001000000000000000;
            prid_o<=32'b00000000010011000000000100000010;
            timer_int_o<=`InterruptNotAssert;
        end else begin
            count_o<=count_o+1;
            cause_o[15:10]<=int_i;
            if(compare_o!=`ZeroWord && count_o==compare_o) begin
                timer_int_o<=`InterruptAssert; 
            end
            
            if(we_i==`WriteEnable) begin
                case(waddr_i)
                    `CP0_REG_COUNT:begin
                        count_o<=data_i;
                    end
                    `CP0_REG_COMPARE:begin
                        compare_o<=data_i;
                        timer_int_o<=`InterruptNotAssert;
                    end
                    `CP0_REG_STATUS:begin
                        status_o<=data_i;
                    end
                    `CP0_REG_EPC:begin
                        epc_o<=data_i;
                    end
                    `CP0_REG_CAUSE:begin
                        cause_o[9:8]<=data_i[9:8];
                        cause_o[23]<=data_i[23];
                        cause_o[22]<=data_i[22];
                    end
                endcase
            end
            case(excepttype_i)
                32'h00000001:begin
                    if(is_in_delayslot_i==`InDelaySlot) begin
                        epc_o<=current_inst_addr_i-4;
                        cause_o[31]<=1'b1;
                    end else begin
                        epc_o<=current_inst_addr_i;
                        cause_o[31]<=1'b0;
                    end
                    status_o[1]<=1'b1;
                    cause_o[6:2]<=5'b00000;
                end
                32'h00000008:begin
                    if(status_o[1]==1'b0) begin
                        if(is_in_delayslot_i==`InDelaySlot) begin
                            epc_o<=current_inst_addr_i-4;
                            cause_o[31]<=1'b1;
                        end else begin
                            epc_o<=current_inst_addr_i;
                            cause_o[31]<=1'b0;
                        end
                    end
                    status_o[1]<=1'b1;
                    cause_o[6:2]<=5'b01000;
                end
                32'h0000000a:begin
                    if(status_o[1]==1'b0) begin
                        if(is_in_delayslot_i==`InDelaySlot) begin
                            epc_o<=current_inst_addr_i-4;
                            cause_o[31]<=1'b1;
                        end else begin
                            epc_o<=current_inst_addr_i;
                            cause_o[31]<=1'b0;
                        end
                    end
                    status_o[1]<=1'b1;
                    cause_o[6:2]<=5'b01010;
                end
                32'h0000000d:begin
                    if(status_o[1]==1'b0) begin
                        if(is_in_delayslot_i==`InDelaySlot) begin
                            epc_o<=current_inst_addr_i-4;
                            cause_o[31]<=1'b1;
                        end else begin
                            epc_o<=current_inst_addr_i;
                            cause_o[31]<=1'b0;
                        end
                    end
                    status_o[1]<=1'b1;
                    cause_o[6:2]<=5'b01101;
                end
                32'h0000000c:begin
                    if(status_o[1]==1'b0) begin
                        if(is_in_delayslot_i==`InDelaySlot) begin
                            epc_o<=current_inst_addr_i-4;
                            cause_o[31]<=1'b1;
                        end else begin
                            epc_o<=current_inst_addr_i;
                            cause_o[31]<=1'b0;
                        end
                    end
                    status_o[1]<=1'b1;
                    cause_o[6:2]<=5'b01100;
                end
                32'h0000000e:begin
                    status_o[1]<=1'b0;
                end
                default:begin
                end
            endcase
        end
    end

    always @ (*) begin
        if(rst==`RstEnable) begin
            data_o<=`ZeroWord;
        end else begin
            case(raddr_i)
                `CP0_REG_COUNT:begin
                    data_o<=count_o;
                end
                `CP0_REG_COMPARE:begin
                    data_o<=compare_o;
                end
                `CP0_REG_STATUS:begin
                    data_o<=status_o;
                end
                `CP0_REG_EPC:begin
                    data_o<=epc_o;
                end
                `CP0_REG_CAUSE:begin
                    data_o<=cause_o;
                end
                `CP0_REG_PRId:begin
                    data_o<=prid_o;
                end
                `CP0_REG_CONFIG:begin
                    data_o<=config_o;
                end
                default:begin
                end
            endcase
        end 
    end
    
endmodule

```



### 中断和例外





## 体系结构优化



### TLB相联存储器

[内容可寻址存储器cam](https://baike.baidu.com/item/%E5%86%85%E5%AE%B9%E5%8F%AF%E5%AF%BB%E5%9D%80%E5%AD%98%E5%82%A8%E5%99%A8/18884435)





### Cache高速缓存

inst_cache

data_cache



# 课外小知识

***

## CPU架构体系

[解析CPU架构体系的区别](https://blog.csdn.net/zhanghuaichao/article/details/89081226?utm_source=app&app_version=4.14.0&code=app_1562916241&uLinkId=usr1mkqgl919blen)

冯诺依曼结构（也称普林斯顿结构）：X86架构

体系核心：数据和指令混在一起，统一编址。区分哪些是指令和哪些是数据大致上有以下方法：

1、用寄存器和指令周期来区分数据和指令。例如：CS段（codesegment代码段）和DS段（datasegment数据段），前者CPU是认为存放的都是指令，后者CPU认为存放的都是数据；

2、通过不同的时间段来区分指令和数据，在取指阶段取出的就是指令，执行阶段取出的就是数据。



其他参考：

[冯诺依曼体系结构](https://www.cnblogs.com/badboy200800/p/12263372.html)：<https://www.cnblogs.com/badboy200800/p/12263372.html>



哈佛结构：ARM架构

核心：数据和指令是区分开的。独立编址，就算地址一样，数据也是不一样的

***



## [CPU的内部架构和工作原理](https://blog.csdn.net/stpeace/article/details/80101441?utm_source=app&app_version=4.14.0&code=app_1562916241&uLinkId=usr1mkqgl919blen)

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

***

## 输入输出接口

接口是CPU与外部设备间的桥梁

分类：

+ 串行接口、并行接口【可编程接口芯片8250、8255】
+ 输入接口、输出接口
+ 数字接口、模拟接口【DAC0832、ADC0809】
+ 中断控制接口【8259A】
+ 存储器接口

功能：

+ <font color=ff0000>速度匹配</font> 数据缓冲寄存buffer【三态门】
+ <font color=ff0000>信号电平或类型的转换</font> A/D D/A
+ <font color=ff0000>信息驱动能力</font> ：实现主机与外设间的运行匹配 电平转换器、驱动器
+ <font color=ff0000>信息格式</font> 字节流、块、数据包、帧
+ <font color=ff0000>时序匹配</font> 定时关系



I/O端口

+ 数据端口
+ 状态端口
+ 控制端口



接口的基本构成：

译码电路       数据输入寄存器/三态门    74LS244

​						 数据输出寄存器/锁存器    74LS273/74LS373

​						 状态寄存器/三态门

控制逻辑       命令寄存器【设定接口功能、工作参数、工作方式】



### Ox01 8088CPU

MN/$\overline{\text{MX} }$：最小模式、最大模式

ALE：地址信号，地址锁存信号，触发器

$\overline{\text{DEN} }$：数据信号，数据使能信号

RESET：复位信号



$A_{0}$—$A_{7}$

$A_{8}$—$A_{15}$

$A_{16}$—$A_{19}$：地址、段寄存器状态复用



IO：访问输入输出接口

$\overline{\text{M} }$：访问存储器

$\overline{WR}$：写信号

$\overline{RD}$：读信号

READY：输入信号，由内存或I/O设备发出



NMI：非可屏蔽中断信号

INTR：可屏蔽中断信号

$\overline{INTA}$：中断响应信号，中断向量码的读选通信号



HOLD：总线保持请求信号输入端，占用总线时向CPU发出的信号

HLDA：总线保持响应信号输出端，CPU对HOLD信号的响应



DT：发送，写操作

$\overline{R}$：接收，读操作



$\overline{SS_{0} }$：系统状态信号输出



> > 内部结构
> >
> > + EU执行单元
> > + BIU总线接口单元
>
> https://zhidao.baidu.com/question/419862964.html
>
> https://blog.csdn.net/fuhanghang/article/details/111571435
>
> https://blog.csdn.net/wujiafei_njgcxy/article/details/52739846

### Ox02 可编程定时器8253

https://blog.csdn.net/qq_42604176/article/details/105810973

https://blog.csdn.net/weixin_42214698/article/details/122515446



### Ox03 可编程并行接口8255A





### Ox04 可编程串行接口8250

## I/O编址方式

https://blog.csdn.net/weixin_42240667/article/details/106256544

+ **与存储器统一编址**

  又称为存储器映射编址方式。它将I/O端口作为[内存](https://so.csdn.net/so/search?q=内存&spm=1001.2101.3001.7020)单元对待，由CPU统一分配地址。通 常在CPU的地址空间中划出一部分作为输入输出系统的端口地址范围，不再作为内存地址使用。

+ **独立编址方式**

  CPU给I/O端口分配一个独立的地址空间，提供专用的控制信号。I/O端口地址空间与内存地址空间隔离。



## PLA（可编程逻辑阵列）

Programmable logic arrays(PLA) 是一种可编程逻辑装置,它的与阵列(AND array)和或阵列(OR array)均为可编程,输出电路为不可组态.又叫做FPLA(field-programmable logic array)

+ 组合可编程逻辑阵列：一个“与”阵列和一个“或”阵列构成

+ 时序可编程逻辑阵列：“与”阵列、“或”阵列和一个用于存储以前状态的**触发器网络**构成

一般首先将给定问题的逻辑函数按多输出逻辑函数的化简方法简化成最简“与-或”表达式，然后根据最简表达式中的不同“与”项以及各函数式的“与”项之和分别构成“与”阵列和“或”阵列，并画出阵列逻辑图。

应用：

用来控制资料路径，在指令集内事先定义好逻辑状态，并用此来产生下一个逻辑状态(通过条件分支)

参考：

<https://wenku.baidu.com/view/70d76b6027d3240c8447efad.html>

拓展：PAL  CPLD  FPGA

## FPGA的内部构造

[【FPGA】FPGA的内部构造](https://blog.csdn.net/u011327754/article/details/79897170)

[FPGA内部硬件结构简介](https://www.cnblogs.com/mc2-xiangliangzi/p/10830661.html)

[FPGA内部结构](https://www.cnblogs.com/lifan3a/articles/4682476.html)

FPGA由6部分组成，分别为可编程输入/输出单元（IOB）、基本可编程逻辑单元（SLENCE）、嵌入式块RAM（block ram）、丰富的布线资源、底层嵌入功能单元和内嵌专用硬核等。

***

## 虚拟地址和实模式



在实模式中，A程序和B程序从磁盘直接加载进内存中运行，我们所看到或者得到的就是物理地址

**我们都知道内存是连续的，现在假设A程序是恶意程序，它现在可以通过自己运行时所获取的物理地址加上偏移就可以进入B程序中，可以篡改B程序的指令或者数据，导致B程序崩溃或者出错**

> 保护模式

在保护模式中，在程序从磁盘加载进内存的中间加了一个中间层，即就是虚拟地址，在程序编译，链接的时候先映射进虚拟地址，在运行的时候会在映射进物理地址

这样的模式好处在于，在虚拟地址中，如图B程序的虚拟地址，不管通过如何偏移，他都在虚拟地址中，最后映射进物理地址也在B程序内，不会影响到其他的程序，起到了进程隔离的作用，保护了其他的进程

**所以如今的寻址方式的模式大都是以保护模式为主，也就是虚拟地址空间，因为其更能保证进程的安全和跟合理的安排程序运行**



原文链接：<https://blog.csdn.net/wfea_lff/article/details/104199529>



***

## [异构计算](https://www.cnblogs.com/missidiot/p/9869275.html)

**CPU、GPU、TPU、NPU、BPU、DPU**

**CPU( Central Processing Unit, 中央处理器)**

是机器的**“大脑”**，也是布局谋略、发号施令、控制行动的**“总司令官**”。 CPU的结构主要包括运算器（ALU, Arithmetic and Logic Unit）、控制单元（CU, Control Unit）、寄存器（Register）、高速缓存器（Cache）和它们之间通讯的数据、控制及状态的总线。简单来说就是：**计算单元、控制单元和存储单元**

CPU遵循的是**冯诺依曼架构**，其核心就是：**存储程序，顺序执行。**



并行计算

并行计算(Parallel  Computing)是指同时使用多种计算资源解决计算问题的过程，是提高计算机系统计算速度和处理能力的一种有效手段。它的基本思想是用多个处理器来共同求解同一问题，即将被求解的问题分解成若干个部分，各部分均由一个独立的处理机来并行计算。

并行计算可分为**时间上的并行**和**空间上的并行**。

时间上的并行是指流水线技术，<font color=0000FF>比如说工厂生产食品的时候分为四步：清洗-消毒-切割-包装。如果不采用流水线，一个食品完成上述四个步骤后，下一个食品才进行处理，耗时且影响效率。但是采用流水线技术，就可以同时处理四个食品。</font>这就是并行算法中的时间并行，在同一时间启动两个或两个以上的操作，大大提高计算性能。

空间上的并行是指多个处理机并发的执行计算，即通过网络将两个以上的处理机连接起来，达到同时计算同一个任务的不同部分，或者单个处理机无法解决的大型问题。<font color=0000FF>比如小李准备在植树节种三棵树，如果小李1个人需要6个小时才能完成任务，植树节当天他叫来了好朋友小红、小王，三个人同时开始挖坑植树，2个小时后每个人都完成了一颗植树任务，</font>这就是并行算法中的空间并行，将一个大任务分割成多个相同的子任务，来加快问题解决速度。



**GPU全称为Graphics Processing Unit**，中文为**图形处理器**

就如它的名字一样，GPU最初是用在个人电脑、工作站、游戏机和一些移动设备（如平板电脑、智能手机等）上运行绘图运算工作的微处理器。

**GPU的构成相对简单，有数量众多的计算单元和超长的流水线，特别适合处理大量的类型统一的数据。但GPU无法单独工作，必须由CPU进行控制调用才能工作**。

GPU中有很多的运算器ALU和很少的缓存cache，缓存的目的不是保存后面需要访问的数据的，这点和CPU不同，而是为线程thread提高服务的。如果有很多线程需要访问同一个相同的数据，缓存会合并这些访问，然后再去访问dram。

GPU不仅可以在图像处理领域大显身手，它还被用来科学计算、密码破解、数值分析，海量数据处理（排序，Map-Reduce等），金融分析等需要大规模并行计算的领域。



ASIC（专用集成电路）

ASIC是指依产品需求不同而定制化的特殊规格集成电路，由特定使用者要求和特定电子系统的需要而设计、制造。当然这概念不用记，简单来说就是**定制化芯片。因为ASIC很“专一”，只做一件事，所以它就会比CPU、GPU等能做很多件事的芯片在某件事上做的更好，实现更高的处理速度和更低的能耗。但相应的，ASIC的生产成本也非常高。**



**TPU（Tensor Processing Unit, 张量处理器）**

就是**谷歌**专门为加速深层神经网络运算能力而研发的一款芯片，**其实也是一款ASIC。谷歌充分意识到了片外内存访问是GPU能效比低的罪魁祸首，因此不惜成本的在芯片上放了巨大的内存。**

另外，**TPU的高性能还来源于对于低运算精度的容忍。**研究结果表明，低精度运算带来的算法准确率损失很小，但是在硬件实现上却可以带来巨大的便利，包括功耗更低、速度更快、占芯片面积更小的运算单元、更小的内存带宽需求等...TPU采用了8比特的低精度运算。



**NPU（Neural network Processing Unit）**， 即**神经网络处理器**

用电路模拟人类的神经元和突触结构

如果想用电路模仿人类的神经元，就得把每个神经元抽象为一个激励函数，该函数的输入由与其相连的神经元的输出以及连接神经元的突触共同决定。

为了表达特定的知识，使用者通常需要（通过某些特定的算法）调整人工神经网络中突触的取值、网络的拓扑结构等。该过程称为“学习”。

**NPU的典型代表有国内的寒武纪芯片和IBM的TrueNorth**

[达芬奇架构NPU](https://zhuanlan.zhihu.com/p/352481421)



**BPU（Brain Processing Unit，大脑处理器）**

由地平线科技提出的嵌入式人工智能处理器架构。第一代是高斯架构，第二代是伯努利架构，第三代是贝叶斯架构。目前地平线已经设计出了第一代高斯架构，并与英特尔在2017年CES展会上联合推出了ADAS系统（高级驾驶辅助系统）



**DPU（Deep learning Processing Unit, 即深度学习处理器）**

最早由国内深鉴科技提出，基于Xilinx可重构特性的FPGA芯片，设计专用的深度学习处理单元（可基于已有的逻辑单元，设计并行高效的乘法器及逻辑电路，属于IP范畴），且抽象出定制化的指令集和编译器（而非使用OpenCL），从而实现快速的开发与产品迭代。事实上，深鉴提出的DPU属于半定制化的FPGA。



***

## 二进制翻译



二进制翻译就是把一种指令集架构的二进制文件转换成用另一种指令集架构中的指令描述的逻辑等价的二进制文件的过程或者技术。比如说把ARM平台的可执行程序利用二进制翻译程序翻译到x86平台。最初的目的是为了满足特定单位的特定用途，现在为了更好的推广国产化一体化平台，使国产平台可以直接运行现有的软件，减小国产平台推广的难度。



解释执行

对源处理器代码中的每条指令实时解释执行,系统不保存也不缓存解释过的指令,不需要用户干涉,也不进行任何优化.[解释器](https://baike.baidu.com/item/解释器)相对容易开发,比较容易与老的体系结构高度兼容,但代码执行效率很差

静态翻译

在源处理器代码执行之前对其进行翻译,将源机器上的二进制可执行[程序文件](https://baike.baidu.com/item/程序文件)A完全翻译成目标机器上的二进制可执行程序文件B,然后在目标机上执行程序B.一次翻译的结果可以多次使用.静态翻译器离线[翻译程序](https://baike.baidu.com/item/翻译程序),有足够的时间进行更完整细致的优化,代码执行效率较高.然而,静态翻译器无法很好地解决[自修改代码](https://baike.baidu.com/item/自修改代码)，间接过程调用及间接跳转等问题,需要依赖[解释器](https://baike.baidu.com/item/解释器)的支持;而且静态翻译器需要终端用户的参与,这给用户使用造成了很大不便。

动态翻译

在程序运行时对执行到的片断进行翻译,克服了[静态](https://baike.baidu.com/item/静态)翻译的一些缺点——如由于不能知道[控制流](https://baike.baidu.com/item/控制流)中某点的寄存器或内存的值,因此不能实现代码挖掘;动态翻译还可以解决大部分实际情况中的自修改代码问题,而这在静态翻译是不可能的;动态翻译可以利用执行时的动态信息来发掘静态[编译器](https://baike.baidu.com/item/编译器)所不能发现的优化机会;动态翻译器对用户可以做到完全透明,无需用户干预。



***

## 性能分析



[Linux 如何迅速分析定位CPU性能瓶颈](https://blog.csdn.net/qq_34556414/article/details/107760105)

> CPU性能指标

**首先，最容易想到的应该是CPU 使用率**，这也是实际环境中最常见的一个性能指标。CPU使用率描述了非空闲时间占总CPU时间的百分比，根据CPU上运行任务的不同，又被分为用户CPU、系统 CPU、等待 I/O CPU、软中断和硬中断等。

● 用户CPU 使用率，包括用户态 CPU使用率（user）和低优先级用户态 CPU使用率（nice），**表示CPU在用户态运行的时间百分比。用户CPU使用率高，通常说明有应用程序比较繁忙。**

● 系统CPU使用率，表示CPU在内核态运行的时间百分比（不包括中断）。系统CPU 使用率高，**说明内核比较繁忙**。

● 等待I/O的CPU使用率，通常也称为 iowait，表示等待I/O的时间百分比。iowait高，**通常说明系统与硬件设备的I/O 交互时间比较长。**

● 软中断和硬中断的CPU使用率，分别表示内核调用软中断处理程序、硬中断处理程序的时间百分比。它们的使用率高，通常说明系统发生了大量的中断。

● 除了上面这些，还有在虚拟化环境中会用到的窃取CPU使用率(steal）和客户CPU使用率（guest），分别表示被其他虚拟机占用的CPU时间百分比，和运行客户虚拟机的CPU时间百分比。

**第二个比较容易想到的，应该是平均负载（Load Average），也就是系统的平均活跃进程数。**它反应了系统的整体负载情况，主要包括三个数值，分别指过去1分钟、过去5分钟和过去15 分钟的平均负载。

理想情况下，平均负载等于逻辑 CPU个数，这表示每个CPU都恰好被充分利用。如果平均负载大于逻辑 CPU 个数，就表示负载比较重了。

**第三个，也是在专门学习前你估计不太会注意到的，进程上下文切换，**包括

● 无法获取资源而导致的自愿上下文切换

● 被系统强制调度导致的非自愿上下文切换。

上下文切换，本身是保证 [Linux](https://so.csdn.net/so/search?from=pc_blog_highlight&q=Linux) 正常运行的一项核心功能。**但过多的上下文切换，会将原本运行进程的CPU时间，消耗在寄存器、内核栈以及虚拟内存等数据的保存和恢复上，缩短进程真正运行的时间，成为性能瓶颈。**

除了上面几种，还有一个指标，**CPU缓存的命中率。由于CPU发展的速度远快于内存的发展，CPU的处理速度就比内存的访问速度快得多。这样，CPU在访问内存的时候，免不了要等待内存的响应。为了协调这两者巨大的性能差距，CPU缓存（通常是多级缓存）就出现了。**



> 性能工具



首先，平均负载的案例。我们先用uptime，查看了系统的平均负载，而在平均负载升高后，又用mpstat和 pidstat，分别观察了每个CPU和每个进程CPU的使用情况，进而找出了导致平均负载升高的进程，也就是我们的压测工具 strss

第二个，上下文切换的案例。我们先用 vmstat，查看了系统的上下文切换次数和中断次数，然后通过  pidstat，观察了进程的自愿上下文切换和非自愿上下文切换情况;最后通过  pidstat，观察了线程的上下文切换情况，找出了上下文切换次数增多的根源，也就是我们的基准测试工具sysbench。

第三个，进程 CPU使用率升高的案例。我们先用 top，查看了系统和进程的CPU 使用情况，发现 CPU使用率升高的进程是 php-fpm;再用 perf top，观察 php-fpm的调用链，最终找出 CPU升高的根源，也就是库函数 sqrt（）。

第四个，系统的CPU使用率升高的案例。我们先用 top观察到了系统CPU升高，但通过 top 和  pidstat，却找不出高CPU使用率的进程。于是，我们重新审视  top的输出，又从CPU使用率不高但处于Running状态的进程入手，找出了可疑之处，最终通过 perf record和 perf  report，发现原来是短时进程在捣鬼。

第五个，不可中断进程和僵尸进程的案例。我们先用 top观察到了iowait 升高的问题，并发现了大量的不可中断进程和僵尸进程;接着我们用  dstat 发现是这是由磁盘读导致的，于是又通过pidstat 找出了相关的进程。但我们用 strace  查看进程系统调用却失败了，最终还是用perf分析进程调用链，才发现根源在于磁盘直接 /O。

最后一个，软中断的案例。我们通过  top观察到，系统的软中断CPU使用率升高;接着查看/proc/softirqs，找到了几种变化速率较快的软中断;然后通过  sar命令，发现是网络小包的问题，最后再用 tcpdump，找出网络帧的类型和来源，确定是一个SYN FLOOD攻击导致的。

另外，对于短时进程，介绍一个专门的工具 execsnoop，它可以实时监控进程调用的外部命令。

***

## 安全机制

[构建CPU级安全机制，龙芯打造自主化时代“新安全观”](http://www.mzfxw.com/e/action/ShowInfo.php?classid=12&id=145238)

[龙芯发布CPU安全体系 从底层防范漏洞](https://baijiahao.baidu.com/s?id=1674229405323556555&wfr=spider&for=pc)



龙芯3A4000/3B4000在设计之初采用了“自主设计”和“安全可信”深度融合的策略，内部集成了漏洞防范设计、硬件国密算法、安全可信模块与安全访问控制机制，在商用密码及等级保护领域具有高安全、低成本、易使用、轻改造和强合规等特点。同时，龙芯3A4000/3B4000也是目前唯一通过国家商密二级型号认证的CPU。



[CPU的运行级别和保护机制](https://zhuanlan.zhihu.com/p/55478625)





***

## 内存计算



[内存内计算](https://baijiahao.baidu.com/s?id=1653678434603844361&wfr=spider&for=pc)





# 拓展

## vivado使用技巧

>vscode配置：方便代码整理、排查和输入

[vscode配置Verilog环境（Vivado+vscode）](https://blog.csdn.net/qq_43066051/article/details/110872386)

>约束文件

FPGA上板过程中对引脚的约束，在上板时使用

参考：

[Vivado开发工具熟悉之XDC约束文件](https://blog.csdn.net/blackcater/article/details/58594515?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0.no_search_link&spm=1001.2101.3001.4242)

[新建约束文件方法](https://blog.csdn.net/weixin_44565714/article/details/108461915)

方法一：新建xdc文件，编写xdc文件
方法二：综合后，open synthesized Design，点击I/O ports 配置管脚约束。

ZYNQ FPGA为减少bit文件大小，提高加载速度常使用以下管脚约束优化

```
set_property BITSTREAM.GENERAL.COMPRESS TRUE [current_design]

set_property CFGBVS VCCO [current_design]

set_property CONFIG_VOLTAGE 3.3 [current_design]`
```



> 将defines.v设置为global include

将defines.v设置成global include，这样的话就可以在别的文件中引用defines.v文件中的内容。而不需要使用`include defines.v。

![2021-10-21-vivado1](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-21-vivado1.png)



>调整仿真时间

将仿真时间由1000ns调整为1000000ns

![2021-10-21-vivado2](https://gitee.com/jie_xue_du/learn_images_ben/raw/master/%20img/2021-10-21-vivado2.png)







## vivado语法补充



>敏感信号列表

参考：

[Verilog系列：敏感信号列表中的运算](https://www.bilibili.com/read/cv12233000)

组合逻辑：输入信号

a or b or c

时序逻辑：时钟信号

posedge clk 时钟信号上升沿

negedge rstn 复位信号下降沿

>关键字

```verilog
$time
//输出当前时钟

$finish
//结束字符

$monitor($time,",a=%b,b=%b,c=%b,result=%b",a,b,c,result)
//屏幕显示输出当前结果

$display("--- iii --- Failed to open file: %s", file_dir_name);
$display("Data process is OK!!!");
//输出相关测试语句

$readmemh("<数据文件名（路径地址和文件名）>",<存储器名>);
//读取指定路径中的文件数据
//在使用这个命令时，”filename”中的路径要用反斜杠’/’，而不是斜杠’\’。
//相对路径不能读取，记得写绝对路径

int          fd_rd ;
open_file("../tb/data_in.dat", "r", fd_rd); 
//文件读写，相对路径里的data内容
$feof(fd_rd)
$fread(read_temp, fd_rd);

input string      file_dir_name ;
input string      rw ;
output int        fd ;
fd = $fopen(file_dir_name, rw);
$fdisplay(fd_rd, "%h", dout);
```

