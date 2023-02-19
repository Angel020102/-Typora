## var  let const ?

1. var  和 let const 的区别？
   - 用var声明的变量存在变量提升，而 let const 不存在变量提升的
   - let 和 const 存在块级作用域，var 没有块级作用域
   - let 和 const 不能重复声明
   - var在全局作用域声明的变量会挂载到window上，它会创建一个新的全局变量作为全局对象的属性
2. let 和 const 区别？
   - const 在声明时就必须有一个初始值，也就是说cosnt 的声明和赋值只能写在一起

- 暂时行死区？
  - JavaScript引擎在扫描代码时发现变量声明时，如果遇到`var`就会将它们提升到当前作用域的顶端，如果遇到`let或const`就会将声明放到TDZ中，如果访问TDZ中的变量就会抛出错误，只有执行完TDZ中的变量才会将它移出，然后就可以正常方法。这机制只会在当前作用域生效。
