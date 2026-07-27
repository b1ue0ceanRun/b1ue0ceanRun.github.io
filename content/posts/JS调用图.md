---
title: Meta learning
date: 2024-03-15T14:52:36+08:00
tags:
  - ML
categroies:
  - ML
draft: true
---
### 论文


JAVA有CHA可以用，因为java有静态类型系统

另一类可选方案是基于流分析（flow analysis）的方法，如 CFA 或 Andersen 指针分析。  
这些方法可以追踪数据（包括函数）在程序中的流动，从而推断调用关系。

- **字段（属性）级分析（field-based）**：
    - 每个属性名只使用一个抽象位置；
    - 因此两个写入同名属性的函数将无法区分，可能作为同一调用目标。
- **仅追踪函数对象**：
    - 不追踪非函数值的流动。
- **忽略动态属性访问**（如 `e[p]` 的形式）：
    - 这些访问在 JavaScript 中极为常见，但静态分析非常困难。
    - 忽略它们会导致不完备，但极大提升可扩展性。



什么是filed-based analysis

*不区分属于不同对象的同名属性，把所有名为 `f` 的属性视为**一个共享的抽象位置**。*

eg:
```
a.f = foo;
b.f = bar;
```

**field-based 分析认为：**
- `a.f` 和 `b.f` 都写入同一个 Prop("f")
- 所以之后遇到 `x.f()` 调用时，可能的目标是 `{foo, bar}`






论文中的 **调用图（Call Graph）** 是一个**静态分析结构**，目标是确定：

> **每个调用点（call site）可能调用哪些函数（function declaration / function expression）。**




### 论文2



### 指针分析

指针分析的输入是：program
输出：variables的指向关系

指针分析一共有一下几种
```
create x = {} : o ∈ [[x]]
assign x = y : [[y]] ⊆ [[x]]
read y = x.f : o ∈ [[x]] => [[o.f]] ⊆ [[y]] 
wirte x.f = y : o ∈ [[y]] => [[y]] ⊆ [[o.f]]
call x = y(a) : f ∈ [[y]] => a ⊆ Param[f], return[f] ⊆ [[x]]
```

eg:

```js
let f = () => {} // func1
let b = (a) = {. // func2
	let h = a.f 
	h();
}
let c = {} // o1
c.f = f
b(c)
```

写约束：
```
func1 ∈ [[f]]
func2 ∈ [[b]]

o1 ∈ [[c]]
[[f]] ⊆ [[o1.f]]

[[c]] ⊆ [[a]]

// 之后计算 函数b中的内容
// 因为没有别的 o ∈ [[a]]

[[o1.f]] ⊆ [[h]]
```

得到的结果
```
f {func1}
b {func2}
c {o1}
a {o1}
h {func1}
o1.f {func1}
```

### 调用图算法

```
L x F
```

Jelly中调用图的定义
```js
/**  
 * Adds an edge in the call graph (both function->function and call->function). */
   
registerCallEdge(
			call: Node, 
			from: FunctionInfo | ModuleInfo, 
			to: FunctionInfo,  
            {native, accessor, external}: {native?: boolean, accessor?: boolean, external?: boolean} = {}) {  
	...
}
```






### cubic算法实现 -- python
本质上是 所有 tokens（obj or functions） T = {t1,t2, ... tn}
和所有 变量 V = {x1,x2,...,xn} 之间的映射关系

我们关注的两个点
1. `t∈x`
2. `t∈x => y⊆z`

确实上一章给的五种都是这种形式
```
create x = {} : o ∈ [[x]]
assign x = y : [[y]] ⊆ [[x]]
read y = x.f : o ∈ [[x]] => [[o.f]] ⊆ [[y]] 
wirte x.f = y : o ∈ [[y]] => [[y]] ⊆ [[o.f]]
call x = y(a) : f ∈ [[y]] => a ⊆ Param[f], return[f] ⊆ [[x]]
```

算法逻辑
```
t∈x: 
	addToken(t,x) 
	propagate() 
t∈x => y⊆z: 
	if t ∈ x.sol then 
		addEdge(y,z) 
		propagate() 
	else 
		x.cond(t).add(y,z) 
	end if
```


可以先实现
`addToken()` & `propagate()`

这里需要搞清楚几个东西：
- x.sol ⊆ T holds the solution for x
- x.succ ⊆ V is the set of successors of x (i.e., the edges of the graph), and 
- x.cond(t) ⊆ V × V represents a set of conditional constraints for x and t
- W ⊆ T × V is a worklist.

x.sol 意思是对于变量 x， 指 x 可能指向哪些 obj
x.succ : 简单来说就是 x 要指向的集合  x = y, y->x
x.cond(t) 处理这种情况 `t∈x => y⊆z`
worklist 由一对 <t,x> 构成，意思是应该吧 t 塞给 变量 x

调用图向我们描述了一个”可达的世界”：

- 入口方法（比如说 `main` 方法）是一开始就可达的；
- 其他的可达方法是在分析的过程中不断发现的；
- 只有可达的方法和语句才会被分析。

定义：
称调用图中，入口方法（Entry Methods）以及从入口方法可达的其他结点为可达方法（Reachable Methods）。所有的可达方法构成了一个可达调用子图（Reachable Sub-Call-Graph）。


## helper functions
natives

为什么 `thePackageObjectToken` 要搞出这么多种？
找到这些的用的地点：


- invokeCallback



### built-in objects

这些对象由 Node 提供，在所有模块中可直接访问。例如：

- global（等同于浏览器的 window）
- process
- Buffer
- setImmediate, clearImmediate
- __dirname, __filename
- module, exports, require
这些对象属于 Node.js Runtime Globals。


我觉得 Function 这个就很有意思

```
call   => 立即调用
bind   => 返回一个新函数但不执行
apply  => 和 call 类似，但参数是数组
```
...


callback???


async 

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/async_function