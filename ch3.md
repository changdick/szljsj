# 时序逻辑电路
### 理解组合和时序电路的含义
时序逻辑是与组合逻辑对应的一个概念，组合逻辑电路的输出完全取决于**此刻**的输入，而时序逻辑电路的输出还受到此前的影响。因此，组合逻辑电路是没有记忆，没有存储功能，而时序逻辑电路有存储的能力，相当于是组合逻辑电路加上存储电路得到的。

|电路构成    |  定义                                             |  结构         |逻辑函数表达式|
|---        |       --------------------------------------------|-------------|--------------|
|组合逻辑电路|电路的输出近与当前时刻输入有关                        |不包含存储元件|只需一组，从输入到一个输出的一个函数|
|时序逻辑电路|电路的输出于当前时刻输入有关，也与上一时刻的工作状态有关|包含存储元件  |有三组|

组合逻辑电路永远是此刻，所以输入输出的表示都是一个字母，但是时序电路要考虑此前的影响，所以输出不能仅仅是一个字母，而是有分次态和现态，加下标，用 $Q_{n+1}$ 和 $Q_n$ 来表示次态和现态。

## 双稳态触发器
### 双稳态触发器特点和解释
1. 有两个互补的输出端 $Q$ 和 $\overline{Q}$。
2. 有两个稳态：0态和1态。 （**稳态指的就是0态或1态**，看输出端Q输出什么，Q输出1是1态，Q输出0是0态。 还告诉我们，一个触发器或锁存器，**能够存储1位二进制数**。换言之，要存储16位二进制数，需要16个触发器。）
3. 在外界信号刺激下，可以从一个稳态变为另外一个稳态。
4. 在没有信号刺激或者无效信号刺激的时候，维持当前状态不变。 （这一点说明记忆功能）

### 基本RS锁存器（或非门实现）

#### 1.电路构成
用或非门实现的基本RS锁存器，或非门的特点是对高电平敏感.
> 如何理解高电平敏感，2输入的不好理解，带“或”的就是高电平1敏感，因为n个输入的或门，只要有1个输入为1，输出就为1， n个输入的或非门，只要有1个输入为1，输出就是1的非，也就是0. 再来看或非门这种命名，“或”和“与”决定了对1敏感还是对0敏感，“非”只是作用给输出，本来至少有一个敏感信号输出1，那么差一个非的时候，至少有一个敏感信号输出0.

由于对高电平敏感，所以有效信号就是1.所谓有效信号，就是R与S不能同时为有效信号，谁为有效信号，就是谁起作用，例如R输入有效信号而S输入无效信号，就置0，若R输入的不是有效信号而S输入有效信号，就置1.
由于触发器或者锁存器都是要求互补的输出端，两个输出端是互补的，所以不允许输入R和S均为有效信号，否则两输出端会不互补。

#### 2.功能表
|置0端R|置1端S|$Q_n$|$Q_{n+1}$|
|---|---|---|---|
|0|0|0|0|
|0|0|1|1|
|0|1|0|1|
|0|1|1|1|
|1|0|0|0|
|1|0|1|0|
|1|1|不允许|不允许|

从功能表可以看出，输入有约束，不允许RS=1， 即要求RS=0。当RS=00时， $Q_{n+1}$总是等于 $Q_n$，说明输入00时为保持。还可以看出为什么R叫置0端，S叫置1端，当置0端R输入1，就变成0态，当置1端S输入1，就变成1态，与电路的现态无关。

功能表的优化：功能表比较大，只有输入保持信号时才与现态有关系，否则只与输入有关系。可以做一个简化。
|置0端R|置1端S|$Q_{n+1}$|
|---|---|---|
|0|0|$Q_n$|
|0|1|1|
|1|0|0|
|1|1|不允许|

#### 3.次态方程
功能表就是一个真值表，所以可以通过功能表，用卡诺图化简，得到一个次态方程，次态方程是描述如何通过现态和输入得到次态。比如这里次态方程就是 $Q_{n+1}=S+\overline{R}Q_n$ ,还附加约束条件RS=0，用卡诺图化简可以得到。但是这并不是非常形象，不好直接想出来。
> 直接想的话比较符合人的描述，当输入00时保持，当输入RS为10时置0，输入01置1，不允许输入11，会写出个 $\overline{S} \overline{R}Q_n + S \overline{R}\cdot 1+R\overline{S}\cdot 0$，化简后一样的

#### 4.驱动表
驱动表关注状态发生不同的变化（一共4种变化），是什么输入驱动的。 描述完成状态转换所需要的输入条件。
|状态变化|R|S|
|---|---|---|
|0 → 0|x|0|
|0 → 1|0|1|
|1 → 0|1|0|
|1 → 1|0|x|

驱动表好用，驱动表是根据状态的变化查询所需要输入是多少。

#### 5.状态图
状态图是有向图。一共两个状态：0态和1态，所以两个状态是对应两个点，有向边表示状态转换，把这一个状态变化所需要的输入取值写在边的旁边，非常形象。

#### 用与非门实现的RS锁存器
与非门对低电平敏感，说明与非门实现RS锁存器，有效信号为0，与非门只要有0就输出1。这种RS锁存器与或非门实现的差别就是有效信号的差别，输入11保持，谁输入有效信号0就起谁的作用，R输入0，S输入1，就置零。  
注意画原理图的区别，或非门的RS锁存器，R输入的或非门输出Q，与非门的RS锁存器，R输入的与非门输出Q。
#### 锁存器的应用
开关去颤。

### 门控D锁存器
有一个使能端，当使能端为0，会保持；当使能端为1，Q的状态会是输入D。 次态方程很好写， $Q_{n+1}=GD+\overline{G}Q_n$。
#### 锁存器的缺点
锁存器有空翻现象，一个时钟周期内，输入信号D可能由于干扰而多次翻转，导致输出状态多次改变，不符合需要。所以需要触发器。
### D触发器
时钟触发器的特点是由时钟脉冲决定转换何时发生，由输入信号决定状态转换如何做。 

D触发器的作用：当时钟信号到来时，输出信号拷贝输入信号。 次态方程： $Q_{n+1}=D$。最简单，应用最广。

### RS触发器
只在时钟信号到时，起功能表、次态方程、驱动表和基本RS锁存器一样。也有输入约束，不允许输入同时有效。
### JK触发器
JK触发器有两个输入端J 和 K。  "JK"与"SR"对应， J会置1，K会置0. 由保持 ，置0，置1，翻转四种功能。
#### 功能表（下降沿）
|时钟信号|J|K|$Q_n$|$Q_{n+1}$|
|-|-|-|-|-|
|↓|0|0|0|0|
|↓|0|0|1|1|
|↓|0|1|0|0|
|↓|0|1|1|0|
|↓|1|0|0|1|
|↓|1|0|1|1|
|↓|1|1|0|1|
|↓|1|1|1|0|

简化功能表
|J|K|$Q_{n+1}$|
|-|-|-|
|0|0|$Q_n$|
|1|0|1|
|0|1|0|
|1|1|$\overline{Q_n}$|

#### 驱动表
|$Q_n$→ $Q_{n+1}$|J|K|
|-|-|-|
|0 → 0|0|x|
|0 → 1|1|x|
|1 → 0|x|1|
|1 → 1|x|0|

#### 次态方程
通过卡诺图方法可以化简出， $Q_{n+1}=J\overline{Q_n}+\overline{K}Q_n$

### T触发器和T'触发器
如果把JK触发器的两个输入端输入同一个信号，就是T触发器，T触发器两个功能，就是保持或者翻转。T=0保持，T=1翻转。若要求其次态方程，那就把T=J=K代入到JK触发器的次态方程，就得到 $Q_{n+1}=T\oplus Q_n$.  
如果把T触发器恒输入1，那就是T'触发器，只有翻转功能，当时钟信号到的时候翻转。显然次态方程为 $Q_{n+1}=\overline{Q_n}$。  
T'触发器可以用来二分频。
### 附加信号的触发器
#### 带附加信号的D触发器
多一个异步清零端和异步置1端，异步就是说独立于时钟信号，起作用与时钟信号无关。二者不能同时输入有效信号。任何时候异步清零端输入有效信号，触发器无条件地置0.不输入异步附加信号的时候，就相当于正常的D触发器。
#### 附加时钟使能端
如果把时钟信号和使能端信号经过一个与门送入时钟端，会出问题，使能端必须不能和时钟信号捆绑使用。可以专门一个使能端，当使能端为1的时候，触发器才正常工作，当使能端为0的时候，就永远保持。或者做一个二选一数据选择器，0端输入现态，1端输入D，再把输出送入D触发器的输入端。当使能端为0的时候，有保持功能，当使能端为1，就会把D送入输入端，发挥D触发器的正常功能。
### 触发器电路的分析
#### 1. 写次态方程
触发器电路经过复杂连接要求写次态方程。  
方法：代数化简法。有些是将输出引到输入端，就直接代入次态方程。
#### 2. 画波形分析
方法1： 先用代数方法求出次态方程，然后再画波形分析。
方法2： 每一个时钟下降沿到来，也就是要做状态转换的时候，逐步计算上去，得到此时的次态。
#### 3. 触发器电路转换
已知一个触发器，但是不是想要的类型，所以要经过一个转换电路变成想要的。例如JK触发器转换成D触发器。  
基本思想：实际上是在求一个转换电路，输入为D，输出为J和K。即J=f(D, $Q_n$),K=g(D, $Q_n$)
方法1：代数方法，通过比较两种次态方程，来得到关系。 $Q_{n+1}=D , Q_{n+1}= J\overline{Q_n}+\overline{K}Q_n$ ,发现D只有一个字母，需要一个技巧进行配平，即 $D=D(Q_n+\overline{Q_n})$，把这个展开之后，通过对比得到J=D,K= $\overline{D}$。  
方法2：卡诺图法。先写驱动表，然后画用D和 $Q_n$求J或K的卡诺图，用卡诺图得到函数。  
其中两类例子，第一类是把JK触发器转换成其他的。
> 例：把JK转成D触发器。
> 必须搞清楚输入输出，两个次态方程，一个是已有的， $Q_{n+1}= J\overline{Q_n}+\overline{K}Q_n$，这个不可以改动。不可以改变已有的触发器。一个是我们目标的，手上的输入端的， $Q_{n+1}=D$，为了用这个D输入得到JK触发器的功能，我们变换D触发器的次态方程，去配平，去拟合JK触发器，然后得到转换关系。

第二类是把D触发器转换成其他的。其实这个特别特别简单。
> 例：把D触发器转换成JK触发器。
> 已有的触发器是D触发器，已有的输入端子是J和K，要用J，K，Q这三个信号去实现D。这时候，有两个方程， $Q_{n+1}=D$和 $Q_{n+1}= J\overline{Q_n}+\overline{K}Q_n$, 不可以尝试去改变D触发器的次态方程，因为这和上面那种情况相反，不是一种情况。我们应该改变的是目标得到的那个次态方程，去拟合已有的触发器，来得到目标的触发器。
> 那这个情况特别简单，因为已有触发器次态方程太简单， $Q_{n+1}=D$，手头的输入端是J和K，目标次态方程不管是什么，之接 $Q_{n+1}$给代入进去，就直接给得到转换关系。
> 所以D转换成JK， $D=J\overline{Q_n}+\overline{K}Q_n$. D转换成RS， $D=S+\overline{R}Q_n$。 D转换成T，就是 $D=T\oplus Q_n$. 全都是直接得到。

第三类是把T触发器转换成其他的，这个因为T触发器转换成别的，用代数法不好做，用卡诺图法好做。


## 时序逻辑电路的分析
时序逻辑电路分两大种类型：分析和设计。分析电路是题目给出一个时序逻辑电路，我们分析这个电路的状态变化，从而得出它的功能。做时序逻辑电路的分析，主要步骤为：写方程，一共写三种方程：驱动方程（如何得到触发器的输入），状态方程（如何得到次态），输出方程（如何得到输出）。然后根据方程，写状态转换表。状态转换表是一种真值表，它描述每一种现态在每一种输入下到次态的变化。 有了**完整的**状态转换表，就可以画出状态转换图，从这个图归纳功能，以及是否能够自启动。  
要点：时序逻辑电路的分析，首先看**是同步还是异步**。如果是同步时序逻辑电路，每一个触发器的状态是同时考虑的。但是如果是异步时序逻辑，就更加复杂，因为要把每一个触发器的时钟信号纳入考虑。在写状态转移方程的时候，要注明每个状态转移方程的触发条件。在画状态转移表的时候，要更复杂。具体步骤是，看全局的外来时钟信号clk，看这个clk接哪个触发器，例如clk接 $Q_1$, 那么每次写次态的时候，先写 $Q_1$的次态，然后写完之后，考虑 $Q_1$从现态到次态的变化，是否会作为边沿信号驱动下一个状态变化。  

状态转移表要写完整，例如输入和现态一共有 $X,Q_3,Q_2,Q_1$，那么写表的时候从0000递增写到1111，全写完再去总结转移规律，画图。    
### 电路的mealy型和Moore型之分
mealy电路的输出值由电路的输入和电路的现态共同决定。 Moore型电路的输入不影响输出，输出仅由现态决定。

## 时序逻辑电路的设计
设计时序逻辑电路是根据实际问题建立一个数字逻辑的模型，去得到相应电路。设计时序逻辑电路的步骤主要为：1、给实际问题设计状态，画出状态转换图，2、画出状态转换表并化简。3、根据状态的数量，确定所需触发器的个数，并按规则分配状态变量。4、画驱动表（状态转换真值表），这一步是从化简后的状态转换表，画出每一个状态的变化，根据每一条状态变化，确定相应的触发器应该是什么输入。5、根据状态转换真值表，对每一个触发器的输入变量，用卡诺图法得到它的逻辑函数。(该变量关于现态和输入的函数)。6、可以画出电路图。7、检查无关项，这是检查没有用到的状态是否可以在任意输入下转换到用到的状态，即能否自启动。如果用m个触发器设计，那么可以表示的状态会有2的m次方个，但实际上不一定用那么多个状态。方法是先写代入写出次态方程，然后把没用的状态代入，输入为每一个取值都检查一遍。
