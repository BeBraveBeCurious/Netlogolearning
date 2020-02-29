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
