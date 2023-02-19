## this的那些事

1. 了解 this 为什么有this
   - 在常见的编程语言中，几乎都有this这个关键字（Objective-C中使用的是self），但是JavaScript中的this和常见的面向对象语言中的this不太一样：
     - 常见面向对象的编程语言中，比如Java、C++、Swift、Dart等等一系列语言中，this通常只会出现在类的方法中。
     - 也就是你需要有一个类，类中的方法（特别是实例方法）中，this代表的是当前调用对象。
     - 但是JavaScript中的this更加灵活，无论是它出现的位置还是它代表的含义。

2. this绑定规则

   - this的绑定和定义的位置（编写的位置）没有关系

   - this的绑定和调用方式以及调用的位置有关系

     - 绑定一：默认的 绑定

       - 独立的函数调用，我们可以理解成函数没有被绑定到某一个对象上进行调用

       - ```js
          // 1.案例一:
         function foo() {
           console.log(this)
          }
          foo()//window
         
         // 2.案例二:
          function foo1() {
            console.log(this)
          }
         
          function foo2() {
            console.log(this)
            foo1()
          }
         
          function foo3() {
            console.log(this)
            foo2()
          }
         
          foo3()//window  window window
         
         
         // 3.案例三:
          var obj = {
            name: "why",
            foo: function() {
              console.log(this)
            }
          }
         
          var bar = obj.foo
          bar() // window
         
         
         // 4.案例四:
          function foo() {
            console.log(this)
          }
          var obj = {
            name: "why",
            foo: foo
          }
         
          var bar = obj.foo
          bar() // window
         ```

     - 绑定二：隐式的 绑定

       - 就是说它的调用位置中，是通过某个对象发起的函数调用（谁调用指向谁）

       - ```js
         //1.案例一:
         var obj = {
           name: "why",
           foo: foo
         }
         
         obj.foo() // obj对象
         
         //2.案例二:
         var obj = {
           name: "why",
           eating: function() {
             console.log(this.name + "在吃东西")
           },
           running: function() {
             console.log(obj.name + "在跑步")
           }
         }
         
         // obj.eating()
         // obj.running()
         
         var fn = obj.eating
         fn()
         
         
         // 3.案例三:
         var obj1 = {
           name: "obj1",
           foo: function() {
             console.log(this)
           }
         }
         
         var obj2 = {
           name: "obj2",
           bar: obj1.foo
         }
         
         obj2.bar()
         ```

         

     - 绑定三：显示的 绑定

       - 必须在调用的对象内部有一个对函数的引用（比如一个属性）

       - 如果没有这样的引用，在进行调用是，会报找不到改函数的错误

       - 正是通过这个引用，间接的将this绑定到这个对象上

       - ```js
         // 1.foo直接调用和call/apply调用的不同在于this绑定的不同
         // foo直接调用指向的是全局对象(window)
         // foo()
         
         var obj = {
           name: "obj"
         }
         
         // call/apply是可以指定this的绑定对象
         foo.call(obj)
         foo.apply(obj)
         foo.apply("aaaa")
         
         
         // 2.call和apply有什么区别?
         function sum(num1, num2, num3) {
           console.log(num1 + num2 + num3, this)
         }
         
         sum.call("call", 20, 30, 40)
         sum.apply("apply", [20, 30, 40])
         
         // 3.call和apply在执行函数时,是可以明确的绑定this, 这个绑定规则称之为显示绑定
         ```

         优先级： 显示 >隐式>默认

3. this 绑定规则之外

   - 忽略显示绑定

     - 如果在显示绑定中，我们传入一个null或者undefined，那么这个显示绑定会被忽略

       ```js
       foo.call(obj)//obj对象
       foo.call(null);//window
       foo.call(undefined);//window
       ```

       

   - 间接函数引用

     - 创建一个函数的间接引用，这种情况使用默认绑定规则

     - ```js
       obj2.foo = obj1.foo //window
       ```

   - ES6箭头函数

     - 箭头函数自身没有this，箭头函数this引用上层作用域中的this

4. 经典面试题

   1. 题一

      ```js
          let name = 222
          let a = {
            name: 111,
            say: function () {
              console.log(this.name)
            },
          }
          let fun = a.say
          a.say()
          fun()
          let b = {
            name: 333,
            say: function (fun) {
              fun()
            },
          }
          b.say(a.say)
          b.say = a.say
          b.say()
      ```

   2. 题二

      ```js
      var name = "window";
      
      var person = {
        name: "person",
        sayName: function () {
          console.log(this.name);
        }
      };
      
      function sayName() {
        var sss = person.sayName;
        sss(); // window: 独立函数调用
        person.sayName(); // person: 隐式调用
        (person.sayName)(); // person: 隐式调用
        (b = person.sayName)(); // window: 赋值表达式(独立函数调用)
      }
      
      sayName();
      ```

   3. 题三

      ```js
      var name = 'window'
      
      var person1 = {
        name: 'person1',
        foo1: function () {
          console.log(this.name)
        },
        foo2: () => console.log(this.name),
        foo3: function () {
          return function () {
            console.log(this.name)
          }
        },
        foo4: function () {
          return () => {
            console.log(this.name)
          }
        }
      }
      
      var person2 = { name: 'person2' }
      
      // person1.foo1(); // person1(隐式绑定)
      // person1.foo1.call(person2); // person2(显示绑定优先级大于隐式绑定)
      
      // person1.foo2(); // window(不绑定作用域,上层作用域是全局)
      // person1.foo2.call(person2); // window
      
      // person1.foo3()(); // window(独立函数调用)
      // person1.foo3.call(person2)(); // window(独立函数调用)
      // person1.foo3().call(person2); // person2(最终调用返回函数式, 使用的是显示绑定)
      
      // person1.foo4()(); // person1(箭头函数不绑定this, 上层作用域this是person1)
      // person1.foo4.call(person2)(); // person2(上层作用域被显示的绑定了一个person2)
      // person1.foo4().call(person2); // person1(上层找到person1)
      ```

   4.  题四

      ```js
      var name = 'window'
      
      function Person (name) {
        this.name = name
        this.foo1 = function () {
          console.log(this.name)
        },
        this.foo2 = () => console.log(this.name),
        this.foo3 = function () {
          return function () {
            console.log(this.name)
          }
        },
        this.foo4 = function () {
          return () => {
            console.log(this.name)
          }
        }
      }
      
      var person1 = new Person('person1')
      var person2 = new Person('person2')
      
      person1.foo1() // person1
      person1.foo1.call(person2) // person2(显示高于隐式绑定)
      
      person1.foo2() // person1 (上层作用域中的this是person1)
      person1.foo2.call(person2) // person1 (上层作用域中的this是person1)
      
      person1.foo3()() // window(独立函数调用)
      person1.foo3.call(person2)() // window
      person1.foo3().call(person2) // person2
      
      person1.foo4()() // person1
      person1.foo4.call(person2)() // person2
      person1.foo4().call(person2) // person1
      
      
      var obj = {
        name: "obj",
        foo: function() {
      
        }
      }
      ```

   5.  题五

      ```js
      var name = 'window'
      
      function Person (name) {
        this.name = name
        this.foo1 = function () {
          console.log(this.name)
        },
        this.foo2 = () => console.log(this.name),
        this.foo3 = function () {
          return function () {
            console.log(this.name)
          }
        },
        this.foo4 = function () {
          return () => {
            console.log(this.name)
          }
        }
      }
      
      var person1 = new Person('person1')
      var person2 = new Person('person2')
      
      person1.foo1() // person1
      person1.foo1.call(person2) // person2(显示高于隐式绑定)
      
      person1.foo2() // person1 (上层作用域中的this是person1)
      person1.foo2.call(person2) // person1 (上层作用域中的this是person1)
      
      person1.foo3()() // window(独立函数调用)
      person1.foo3.call(person2)() // window
      person1.foo3().call(person2) // person2
      
      person1.foo4()() // person1
      person1.foo4.call(person2)() // person2
      person1.foo4().call(person2) // person1
      
      
      var obj = {
        name: "obj",
        foo: function() {
      
        }
      }
      ```

   6.  题六

      ```js
      var name = 'window'
      
      function Person (name) &#123;
        this.name = name
        this.obj = &#123;
          name: 'obj',
          foo1: function () &#123;
            return function () &#123;
              console.log(this.name)
            &#125;
          &#125;,
          foo2: function () &#123;
            return () => &#123;
              console.log(this.name)
            &#125;
          &#125;
        &#125;
      &#125;
      
      var person1 = new Person('person1')
      var person2 = new Person('person2')
      
      person1.obj.foo1()() // window
      person1.obj.foo1.call(person2)() // window
      person1.obj.foo1().call(person2) // person2
      
      person1.obj.foo2()() // obj
      person1.obj.foo2.call(person2)() // person2
      person1.obj.foo2().call(person2) // obj
      ```

   

一些关于this的补充

1. 隐式丢失
   - 变量赋值
   - 参数赋值      在预编译的过程中，实参被赋值为形参（值的拷贝过程，浅拷贝）
2. 父函数有能力决定子函数的this指向

Javascript预编译原理

1. 语法分析
   - 先全部扫一遍 看有没有语法错误.
2. 预编译（执行前一刻）
   - 变量 声明提升 函数声明整体提升
3. 解释代码
   - 解释一行执行一行

- **函数中:预编译执行四部曲**
  1. 创建AO对象 (Activation Object (执行期上下文))
  2. 找形参和变量声明,将变量和形参名作为AO属性名,值为undefined
  3. 将实参值和形参统一
  4. 在函数体里面找函数声明,值赋予函数体 **全局中:预编译三部曲**
  5. 创建GO对象(Global Object window就是全局)
  6. 找变量声明,将变量声明作为GO对象的属性名，值赋予undifined
  7. 找全局里的函数声明，将函数名作为GO对象的属性名，值赋予函数体

疑问

1. 立即执行函数中的this指向哪里? 严格模式下呢？

   - 指向window 
   - 严格模式指向 undefined

2. 闭包的this指向哪里？

3. 实例对象，原型，构造函数?

   