 # 逻辑电路设计
 ## 逻辑电路设计的方法
 先分析得到逻辑函数，得到最简形式，在转换成电路形式。
 ## 用单一逻辑门设计多级门电路
 看逻辑函数什么形式：  
 **与或式**-两次取反展开一次-**单与非门**  
 **或与式**-两次取反展开一次-**单或非门**  
 用单一与或非门设计逻辑电路：第二级当作非门使用。
 ## 多输出电路的设计
 当电路有多个输出值时，每个输出值的逻辑函数都设计成最简不一定得到总体的最简，可以寻找共享项，达到总体最简。寻找共享项可以用代数法，也可以在卡诺图中找一样的圈。
 ## 用有限扇入门设计电路
 扇入系数：门电路允许的输入数量。  
 如果对扇入系数做了限定，那么本来的二级门电路可能就会扇入系数受限，为了能够设计电路，需要**增加级数**。
 ## 组合逻辑元件
 ### 1 多路开关/数据选择器
 分为输入数据端、选择控制端、输出端，选择控制端输入数为n，那么输入数据端的输入数就为2的n次方，因为n个信号可以控制的组合就是2的n次方种，刚好一一对应。。 输出端只有1个输出。对于选择控制端的每一种输入组合，会有对应的一个输入数据从输出端输出。  
 输出Z的逻辑函数就可以写成一堆积之和的形式。
 #### 多路选择器设计组合逻辑电路
 用多路选择器设计组合逻辑电路有几种情况，数据选择器的输入端比组合电路的输入变量个数多时，就需要用到降维技巧。
 ### 2 三态门
 三态门有一个使能端，使能端输入控制它有效的信号时才能工作，使能端输入控制它不工作的信号时就是高阻态相当于不在电路中。
 #### 用三态门设计组合逻辑电路
 输入8421BCD码，从中选择可以被5整除的输出，或者选择大于6的数字输出。  
 设计方式：用三态门连总线的方式，用一个总线控制是否输出。是否输出的条件就是用真值表、卡诺图得到逻辑函数，然后得到电路。这个电路出来的输出就是总线。如果用低电平有效三态门，就要对逻辑函数取反。
 ### 3 二进制译码器
 输入端和输出端都是多个，输入端有n个，输出端就有 $2^n$ 个，输入端是一个二进制数，输出端唯一对应的那一个输出会呈现有效输出。又叫N中取一译码器。典型的时3-8译码器，3个输入，8个输出。  
 #### 地址译码问题
 利用译码器可以实现外设选择的功能，输入端有若干个输入信号，输入不同的输入信号时，输出端会有且仅有一个有效信号，这个有效信号可以用于选择某个外设。左边的输入信号被称为地址，做法就是保证使能端输入确定，然后确定3-8译码器的输入信号，后面与输入信号无关的地址位可以任意。
 #### 译码器设计电路
 用3-8译码器设计全加器、全减器。先写真值表，真值表可以一目了然的看出输出为0-7时哪个输出会是有效的，可以直接得到最小项表达式 $\Sigma m(...)$,里面数字是几，就直接把几号输出引出来做个与非门或者或门。
### 4 编码器
编码器与译码器是互逆过程。
### 5 奇偶校验器
奇偶校验器是
