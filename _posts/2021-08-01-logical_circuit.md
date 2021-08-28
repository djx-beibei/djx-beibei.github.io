---
title: logical circuit
categories: [longxin_nscscc]
comments: true
---
>>龙芯杯开源代码：<http://cpu.csc-he.com/a/shiyanshebei/2019/0902/59.html>

[“龙芯杯”全国大学生计算机系统能力培养大赛——信息汇总](https://github.com/loongson-education/nscscc-wiki)

# 常见逻辑电路

>语言基础：verilog

>使用软件：vivado 2019.2

>用途：深入理解掌握数字电路和Verilog语法

>参考书籍：CPU设计实战

目录：

[基础逻辑门](##基础逻辑门)

[译码器](##译码器)

[编码器](##编码器)

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



[Verilog 教程基础篇](https://www.runoob.com/w3cnote/verilog-tutorial.html)
[Verilog 教程高级篇](https://www.runoob.com/w3cnote/verilog2-tutorial.html)