---
title: Meta learning
date: 2024-03-15T14:52:36+08:00
tags:
  - ML
categroies:
  - ML
draft: true
---



想一下要怎么看
首先找到


- variables
- token
- constraint variables

既然远哥说他写的比我好，那就先看看他写的：
```js
function programVarHasToken(id: string, t: string, f: FragmentState): boolean {  
    let findVar = false;  
    for (const constraintVar of f.vars) {  
        if (constraintVar instanceof NodeVar && isIdentifier(constraintVar.node) && constraintVar.node.name===id) {  
            logger.info(`Found var: ${constraintVar}`);  
            for (const token of f.getTokens(constraintVar)) {  
                logger.info(`${token}${token.nativeName ? ` (${token.nativeName})` : ""}`);  
                if ((token.nativeName && token.nativeName.startsWith(t)) || token.toString().startsWith(t)) {  
                    return true;  
                }  
            }  
        }  
    }  
    assert(findVar, `Variable ${id} not found in fragment state`);  
    return false;  
}
```


```js
let fs = require('fs');  
let fsreadFileSync = fs.readFileSync;  
let fsappendFile = fs.appendFile;
```

想到于把

O.fi -> X 

`FragmentState` 是什么数据类型？

Analysis state for a fragment (a module or a package with dependencies, depending on the analysis phase).




registercallgraph

constraintVars


tokens RVT -- 指针集合
```js
/**  
 * The set of constraint variables (including those with tokens, subset edges, or listeners, but excluding those that are redirected). */
readonly vars: Set<RVT> = new Set;
```



```js
/**  
 * A constraint variable for an AST node. 
 */
   
export class NodeVar extends ConstraintVar {  
  
    constructor(readonly node: Node) {  
        super();  
    }  
  
    toString(): string {  
        return nodeToString(this.node);  
    }  
  
    getParent(): Node {  
        return this.node;  
    }  
  
    getKind(): string {  
        return isIdentifier(this.node) ? `Identifier[${(this.node as any)[IDENTIFIER_KIND]}]` : this.node.type;  
    }  
}
```


这个 `isIdentifier` 很重要



export\[fs\].y 指向 objectpropertyVar
ObjectPropertyVar 0.j. --> accessor


require('xxxx')

为什么export 可以是。 o2 属于 o1.f。


O2 属于 export\[fs\].f


拿export 的fields。 objectproperties： map objectpropertyvardb


指向关系
```js
readonly objectProperties: Map<ObjectPropertyVarObj, Set<string>> = new Map;
```



会议总结


指针分析三个基本元素
- Variable
- Object
- Constraint

variable object 
constraint variable



- true 或 false 都返回 false
- 遇到 if else  return true. o1?o2


objects = tokens
pointer = constraint variable

∈


mock需要注意的点

- 参数 和 return 必须正确（对应constraint和object）



所以我要是想写MOCK的Test
我应该做什么？？？



我们现在有的
- Node文档
- Type/node 源码
- Node 源码
- 反射（但这个更多是从测试的角度去考虑）

只关注四种
New x = new T()
Assign x = y
Store x.f = y
Load y = x.f
Call r = x.k(a, ...)
### Watchfile为例

callback 的指针分析




指针分析中是没有函数，类的概念的。那怎么去理解比如Nodejs中的fucntion和class呢？
指针分析只关注
- variables
- object
- points-to
**指针分析并不关心语法层面的结构**（比如“这是一个类方法”还是“普通函数”），因为最终在内存层面它们都表现为：
> 某个函数对象 / 某个闭包 / 某个对象的引用。

举个例子：
```
function foo() {}         // 函数声明
class Bar {}              // 类声明
const baz = () => {}      // 箭头函数
```




Synchronous API


设计Agent




如何检验mock的对不对？
如何指针分析 --> 污点分析

带着疑问？


- taint
- source
哪些变量或对象受到了外部输入（tainted），它们是否被传入敏感操作（sink）。

而标准库往往是：
- **source**：读取用户输入或外部数据；
- **sink**：执行命令、写文件、发网络请求；
- **bridge**：在异步、事件或流之间传递对象引用。

通过token传播







### natives代码阅读

assignParameterToThisProperty

结合指针分析
回顾一下andersen指针分析

收拾旧山河
重新总结 上面的对应关系


拿个例子来看

直接把emit写了

从AST的角度进行理解



