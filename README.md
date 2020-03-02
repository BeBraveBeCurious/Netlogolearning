# Netlogo-learning

[生命游戏认识Patch](#生命游戏认识patch) 
[初始设置：初始添加setup按钮](#初始设置：初始添加setup按钮) 
[出生or死亡规则设置](#出生or死亡规则设置) 
[与python编程语言的的区别](#与python编程语言的的区别) 
 
## 生命游戏认识Patch
- 元胞自动机

### 初始设置：初始添加setup按钮
- ask 相当于 普通编程语言中 for
- set 相当于赋值
 set pcolor white 赋值为白色
 
### 出生or死亡规则设置
- 使用 patch living属性
- 使用patches-own[…] 来定义patch的自定义变量，本例定义living属性，可以在[...]定义多个属性

### 与python编程语言的的区别
- 函数调用 使用空格 func var1 var2
i.e., set living count neighbors with [pcolor = black]
```
set living = (count (neighbors with [pcolor = black]))
count统计括号内的函数的个数
tt=0;
for each patch in neighbors{
    if patch.pcolor==black{
       tt=tt+1;
    }
}
living = tt
```
- count函数使用
- 赋值语句 set 
- 对象集合 patches turtles
- 大于小于等符号 要加空格
- ifelse基本语法

```
ifelse condition [
    *expression1*
][
    *expression2*
]
```
- 生命游戏需要在界面中显示更多的patches，edit即可


## Langton蚂蚁：关联turtles和patches

### turtle与patch的关系
- 一个turtle必然对应一个patch
- 一个patch可以对应多个turtles
- 也存在patch没有对应的turtles的情况
- 总结： turtle看作自变量的集合，patch看作映射得到的值，

### heading
- 设置蚂蚁🐜的行进方向
- 直接使用了Patch的属性，Turtle无此属性，但因Turtle和Patch的关系，可直接调用
- 初始随机化时使用Random 随机生成**整数**函数

### random
- random x 根据x的正负生成，正整数，0 or 负整数， 0
- random-float 1 生成1个0-1的随机小数

### right
- 根据判断条件对蚂蚁行进方向进行 顺时针修改
- similarly, 逆时针修改可 left 90 or right (- 90)

### 关于ticks
- 对蚂蚁移动次数进行记录
- setup 中添加语句 reset-ticks
- go 语句中添加语句 ticks
- 修改 连续更新 为 **按时间进行更新**
- 10000步==吸引子，总是在10000步左右修建出高速公路


## 羊-草生态系统:Turtle与Plot画图
- 典型的捕食者与被捕食者的系统

### 羊-草生态系统规则描述
#### 一个由羊（Turtle）和草（Patch）两种物种构成的小型生态系统
- 羊的内部有一个能量值水平 
- 吃掉草可以增加能量值 set energy energy + 10
- 每一个周期都在消耗能量 turtle_move
- 能量值小于等于0，就会死掉 turtle_die
#### 羊能够繁殖 turtle_breed
- 当能量聚积到一定水平之后，就会繁殖 if energy > 500
- 繁殖需要消耗能量 
- 新出生的羊会天然具备一定的能量
#### 草是可以自发地从地里长出来的 add_food

### 使用函数调用子函数形式来实现 move breed die
```
addfood
ask turtles [
    turtle_move
    turtle_breed
    turtle_die
]
```

#### 关于方法和属性的上下文==一个属性变量的主体（上下文）
- turtle_move  turtle_breed turtle_die 是在ask turtles 函数内部的
- 自动调用turtles所在的patch的属性
- 每一个turtle必对应一个patch
- 如果在ask turtles 外部使用子函数，则会报错

### 如何追踪某一个具体的Turtle和Patch
- 右键选择可有inspect， 含多项具体参数
- watch，光环buff加持
- follow，按所followed对象移动视图，使其为中心

### Plot 绘图
- 添加按钮 下拉选择
- 绘图笔名称使用English，作为函数需要调用，中文易出错
- tick 使用才能覆盖仿真系统原有的时间钟
- reset-ticks in setup; tick in go
- 自动调整图形尺度
- 显示图例

## 经济系统by turtles互动
### 问题描述： 财富分布的不平等性
- 意大利经济学家Pareto早在19世纪就发现，财富或收入的分布满足帕累托分布
- 通俗说，存在着“二-八”准则

### 一个最简人工经济模型
- 2000年，物理学家Victor M. Yakovenko提出了一个最简单的人工经济模型：货币转移模型，通过eplison随机数进行转移实现
- 一个经济系统中人和财富总量保持不变 = 从观察者角度输入命令可查看 sum [money] of turtles; count turtles 判断系统逻辑是否正确
- 开始的时候，每个人都有等量的货币 set money (total_money / num_agents)
- 每当两个Agent相遇，它们俩就随机分布财富 deltam 进行再分布财富

### 
- 如何使用滑块控件
 - num_agents 
 - total_money
- Let和Set两种赋值语句的区别
 - set 只能给已定义的变量进行赋值
 - let 初始化未定义的变量 + 赋值，同时操作 i.e., agsets
- one-of, n-of的使用
 - 从集合agentset中随机选择一个元素
 - 从集合agentset中随机选择n个元素
- 如何访问另一个Agent的内部状态 ask trader
``` ask turtles[
    let agsets other turtles
    if count agsets >= 1
    [
      transaction (one-of agsets)
    ]
    forward 1
  ]

to transaction [trader]
  let deltam 0
  let money1 ([money] of trader)
  let epsilon (random-float 1)
  set deltam (epsilon - 1) * money + epsilon * money1
  if money + deltam >= 0 and money1 - deltam >= 0
  [
    set money money + deltam
    ask trader[
      set money money1 - deltam
    ]
    
  ]
end
([money] of trader)为agent2的作用域，其余为agent1的作用域
set money money1 - deltam 为agent2的作用域，其余为agent1的作用域
```
- 绘直方图的方法
 - 更改绘笔属性为条形
 - 绘图更新命令 update-plot
 - reset-ticks in to setup, tick in go, 按时间步进行更新
 - 自定义绘图更新函数，增加lst列表，得到turtles的money属性值，非空，设定x [0, max lst]值
``` 
let lst [money] of turtles
set-histogram-num-bars 100
if not empty? lst [
  set-plot-x-range 0 max lst
  histogram lst
]
```
## 待解决question：为什么let agsets other turtles-here和 let agsets other turtles导致直方图y轴的差异
