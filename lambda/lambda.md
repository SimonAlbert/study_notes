## lambda calculus
### 1.1 λ演算/lambda calculus
数学上lambda演算用来表示一个可计算函数,
#### 1. lambda项
lambda项就是一个变量/一个抽象或者一个应用
#### 2. 变量
变量是由任意字符组成的字符串
#### 3. 抽象
由一个变量和一个lambda项组成,`λ'变量'.'λ项'`
#### 4. 应用
一个应用由隔开的两个lambda项构成,形如M N(这里的M/N都是lambda项)  
他们的定义表达是递归的  
更多计算理论知识参见:[计算理论基础](https://oi-wiki.org/misc/cc-basic/)  

函数`f(x)=x+1`可以表示为`λx.x+1`,`f(3)`可以表示为`(λx.x+1) x`  
函数`f(x,y)=x-y`可以表示为`λx.λy.x-y`  
`f(7,2)=(λx.λy.x-y) 7 2=(λy.7-y) 2`  
表达式递归定义
```
<expression> := <variable> | <function> | <application>
<function>   := λ <variable>.<expression>
<application> := <expression> <expression>
```  
### 1.2 λ演算的基础操作
#### 1. α-变换
可以在不改变抽象所代表的含义的前提下,改变抽象中的变量(也就是"λ"和"."之间的部分)的  
内容  
```
λx.x→λy.y
```  
#### 2. β-归约
β-归约是指将一个应用中出现的左右两个λ项,用替换的方式把右侧合并到左侧最终只剩下一  
个λ项的操作  
```
(λx.x+2) 2 = 2+2
```  
#### 3. η-变换
η-变换是指两个不同的λ项如果各自经过α-变换/β-归约,最后能得到意义完全一样的λ项,那么  
这两个λ项就是等价的  
如果函数f的参数或者返回值是函数,那么f是一个高阶函数(higher－order function)  
> 可以证明,用类似的方式可以把任意多参数的函数都转化为单参数的高阶函数,这种转化又叫做柯里化(Currying)  
### 1.3 邱奇数(Church numerals)
 
#### 1. 邱奇编码(Church Encoding)
邱奇编码是邱奇给出的一种编码方式,用来在λ演算中表示数值和运算符  
在λ演算中,唯一能定义的就是函数.因此只能借用函数进行信息的编码,在邱奇编码中,数值  
和运算符都被映射为高阶函数,不需要从λ演算之外引入其他的概念.可以证明,邱奇编码是  
图灵完备的,因此通过它可以解决任何可以计算的问题,这在理论上证明了λ演算的可用性  
引用[知乎:λ演算 - 函数式语言的起源](https://zhuanlan.zhihu.com/p/164700404)
#### 2. 邱奇编码中的自然数定义
0 := λf.λx.x  
1 := λf.λx.f x  
2 := λf.λx.f (f x)  
3 := λf.λx.f (f (f x))  
......  
n := λf.λx.f (f (... (f x)...)  ...n个f  
邱奇数n是一个高阶函数  
```
n f x = λf.λx.f (f (... (f x)...) f x → λx.f (f (... (f x)...) x → f(f(...(f x)...))
```
> 不好理解,因为这里涉及到自然数的定义,此处的自然数并非我们直观的1234  
> 感谢舒航@知乎的[Understanding Church numeral 如何理解邱奇数](https://zhuanlan.zhihu.com/p/504190912)  
> **皮亚诺公理 Peano axioms**
> (好家伙,皮亚诺公理都出来了)
> 1. 存在自然数0
> 2. 每个自然数都有一个后继数,后继数也是自然数
> 3. 任何自然数的后继数都不是0
> 4. a的后继数=b的后继数,则a=b
> 5. 若集合S中全是自然数,且满足两个条件:(1)0在集合S中.(2)若任给实数n在集  
> 合S中，那么n的后继数n'也在S中,那么S是包含全体自然数的集合  
>   
> 所以,邱奇数就是完美契合lambda演算法的自然数  
> 邱奇数的0是λf.λx.x  
> 邱奇数的后继函数(求自然数的后继数的函数)是`λnSz.S(nSz)`  

#### 3. 邱奇数的运算
理论上,使用邱奇编码可以解决所有可以计算的问题
(先不整理了,给祖师爷们跪一个)
//TODO  
  
后面的内容为个人总结,部分可能有误甚至胡扯
## lambda表达式  
lambda表达式是一个匿名函数,直接对应lambda演算中的lambda抽象  
lambda表达式可以让代码更简洁,更优雅
## 函数式编程
### 正经的函数式编程语言
Haskell,Lisp等  
//TODO  
原来我还没学过真正的函数式编程语言啊(这下流汗黄豆了)
### ~~不正经的函数式编程语言~~支持lambda语法的语言
很多oop语言支持lambda语法,但不一定支持所有场景  
JavaScript中使用lambda  
```JavaScript
const zero = f=>x=>x
const one = f=>x=>f(x)
const tree = f=>x=>f(f(x))
... ...
const toNumber = n=>n(i=>i+1)(0) //将邱奇数转换为自然数01234
```
上面的写法完全符合邱奇数的定义,但是这样没法写出后继函数的定义  
改成如下形式:  
```
const zero = (f,x)=>x
const one = (f,x)=>f(x)
const two = (f,x)=>f(f(x))
... ...
const toNumber = f=>f(x=>x+1, 0)
const successor = (s)=>(f,x)=>f(s(f,x))
console.log(toNumber(zero))
console.log(toNumber(successor(zero)))

```
Java中使用函数式接口@FunctionalInterface注解的接口(有且只有抽象方法)可以被隐式转换为lambda表达式  
相比JavaScript,Java Lambda的适用场景更少,但已经足够极大的改善编程体验  
语言还是那个OOP语言,但是通过支持Lambda语法可以带来部分函数式编程的体验
