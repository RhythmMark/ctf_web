# 笔记
## 逆向工程——汇编基础（一）  
### 汇编语言  
一种最接近计算机核心的编码语言。 汇编语言就是机器语言的一种可以被人读懂的形式，只不过它更容易记忆。 
#### 处理器（cpu）
汇编语言被编译成机器语言之后，将有处理器（CPU）来执行。
- 典型处理器功能</br>
1. 从内存获取机器语言指令，译码，执行。  
2. 根据指令代码管理寄存器。  
3. 根据指令或自己的需要修改内存内容。  
4. 响应其它硬件的中断请求。   
#### 寄存器  
寄存器位于cpu中，保存特定长度的数据。寄存器可以被装入数据，可以在不同寄存器之间移动这些数据，或者做类似的事情，如四则运算、位运算等操作。  
#### 通用寄存器  
- EAX：32-bit。常用于①进行运算，②保护模式中作为内存偏移指针 （此时，DS作为段寄存器或选择器）。  
- EBX：32-bit。常用于①内存偏移指针（DS是默认的段寄存器或选择器）②保护模式中和EAX相同。  
- ECX:32-bit.常用于①用于特定指令的计数②保护模式中和EAX相同  
- EDX: 32-bit。常用于①在一些运算中作为EAX的溢出寄存器（eg：乘除）②保护模式中和EAX相同  
- ESI: 32-bit。常用于①内存操作指令中作为源地址指针使用②可被装入任何数值制、但通常不用作寄存器。DS作为寄存器或段选择器  
- EDI:32-bit。常用于①内存操作指令中作为目的指针使用。  
- EBP: 32-bit。常用于①指针寄存器，通常，它被高级语言编译器用以建造堆栈帧来保存函数或过程的局部变量。②ss是它的默认段寄存器或选择器。   
***注意：这三个寄存器没有对应的8-bit分组。但可以通过SI、DI、BP分别访问他们的低16位。***  
#### 段寄存器和选择器  
实模式下的段寄存器到保护模式下，摇身一变就成了选择器。不同的是，实模式下的段选择器是16-bit的，而保护模式下的选择器是32-bit的。  
- CS 代码段

CS，代码段，或代码选择器。同IP寄存器一同指向当前正在执行的地址。处理器执行时从这个寄存器指向的段（实模式）或内存（保护模式）中获取指令。除了跳转或其他分支指令外，你无法修改这个寄存器的内容。

- DS 数据段

DS，数据段，或数据选择器。这个寄存器的低16-bit连同ESI一同指向指令将要执行的内存。同时，所有的内存操作指令默认情况下都用它指定操作段（实模式）或内存（保护模式下作为选择器）。这个寄存器可被装入任意数值，做法是先把数据给AX，在把它从AX传送给DS。当然也可通过堆栈来做。

- ES 附加段

ES，附加段，或附加选择器。这个寄存器的低16-bit连同EDI一同指向指令将要处理的内存。其他同DS。

 - FS

FS，F段或F选择器。可以用这个寄存器作为默认段寄存器或选择器的一个替代品。

- GS

GS，G段或G选择器。它和FS几乎完全一样。

- SS

SS，堆栈段或堆栈选择器。这个寄存器的低16-bit连同ESP一同指向下一次堆栈操作（push和pop）所要使用的堆栈地址。这个寄存器也可以被装入任意数值，可通过入栈和出站操作来赋值。</br>
***注意，一定不要在初学汇编阶段把这些寄存器弄混。段寄存器或选择器，在没有指定的情况下都是使用默认的那个。这句话在现在看来可能有点稀里糊涂，不过你很快会在后面知道如何去做。***
#### 特殊寄存器

- EIP

EIP，32-bit，这个寄存器非常重要，同CS一同指向即将执行的那条指令的地址。不能够直接修改这个寄存器的值，修改它的唯一方法是跳转或分支指令。（CS是默认的段或选择器）

- ESP

ESP，32-bit，这个寄存器指向堆栈中即将被操作的那个地址。尽管可以修改它的值，但并不提倡这样做，可能会破坏堆栈。（SS是默认的段或选择器）<br>
IP： Instruction Pointer，指令指针。<br>
SP： Stack Pointer，堆栈指针。<br>
#### 标志寄存器
![sd](http://m0nst3r.me/usr/uploads/2017/11/1975533938.png)
#### 其他寄存器
CR0、CR2、CR3（控制寄存器）。例如CR0的作用是切换实模式和保护模式。<br>
D0、D1、D2、D3、D6和D7（调试寄存器），他们可以作为调试器的硬件支持来设置条件断点。<br>
TR3、TR4、TR5、TR6、TR?寄存器（测试寄存器）用于某些条件测试。        
