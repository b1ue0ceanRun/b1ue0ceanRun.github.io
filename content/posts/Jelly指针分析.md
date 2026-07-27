---
title: Meta learning
date: 2024-03-15T14:52:36+08:00
tags:
  - ML
categroies:
  - ML
draft: true
---
x = y → alias[x] includes y


解释 AccessPath


论文中的建模：
```
AccessPath ::=  < ImportPath > 
| Fun⟨f, l⟩ 
| Fun⟨f, l⟩.Param[i] 
| AccessPath . Prop 
| AccessPath (P (AccessPath), . . . ) 
| U
```

模块加载： `<m>`
```js
require("lodash")
```

函数定义： `Fun⟨file, line⟩`
```
function f() { ... }
const g = () => { ... }
```

形式参数： `Fun⟨file,line⟩.Param[i]`

```
Fun⟨file, line⟩.Param[0]
Fun⟨file, line⟩.Param[1]
```

字段访问：`ap.Prop`

```
obj.f
obj["name"]
```
函数调用： `ap(args…)`

AccessPath 不是 points-to 集合，但扮演等价角色


AccessPath 是 Jam 中所有“可能被调用的函数值”的抽象标识符。  
它负责追踪函数值在模块、属性、参数、返回值中传播的路径，并最终告诉系统：  
“这个调用表达式代表的函数可能是哪一个”。

最终每一条调用边 `(caller → callee)` 都是由 AccessPath 匹配推导出来的。



Jam **不区分不同对象实例**，使用 field-based（字段敏感，object-insensitive）


# 为什么需要 AccessPath？

JavaScript（尤其是 Node.js）极度动态：

- 没有类型
- 函数是一等对象
- 返回函数 / 参数是函数常见
- 对象属性动态添加
- require 动态返回 module.exports 对象
- 动态 field 访问 obj[prop]


## **给每个函数值一个统一的“身份标识”**

JavaScript 中：

- 函数可赋值给变量
- 函数可存在对象属性里
- 函数从函数中返回
- 函数可作为参数传递
- require() 动态生成 module.exports 对象
- 函数可被包装、嵌套、currying、闭包……

如果没有一种统一的“抽象值表示法”，则无法追踪哪个 callsite 调的是哪个 function definition。