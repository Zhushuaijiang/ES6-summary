#ES6 迭代器
    Iterator
    Iterator 是 ES6 引入的一种新的遍历机制，迭代器有两个核心概念：

    迭代器是一个统一的接口，它的作用是使各种数据结构可被便捷的访问，它是通过一个键为Symbol.iterator 的方法来实现。
    迭代器是用于遍历数据结构元素的指针（如数据库中的游标）。

    迭代过程
    迭代的过程如下：

    通过 Symbol.iterator 创建一个迭代器，指向当前数据结构的起始位置
    随后通过 next 方法进行向下迭代指向下一个位置， next 方法会返回当前位置的对象，对象包含了 value 和 done 两个属性， value 是当前属性的值， done 用于判断是否遍历结束
    当 done 为 true 时则遍历结束

    const items = ["zero", "one", "two"];
    const it = items[Symbol.iterator]();
    it.next();
    >{value: "zero", done: false}
    it.next();
    >{value: "one", done: false}
    it.next();
    >{value: "two", done: false}
    it.next();
    >{value: undefined, done: true}
    上面的例子，首先创建一个数组，然后通过 Symbol.iterator 方法创建一个迭代器，之后不断的调用 next 方法对数组内部项进行访问，当属性 done 为 true 时访问结束。

    迭代器是协议（使用它们的规则）的一部分，用于迭代。该协议的一个关键特性就是它是顺序的：迭代器一次返回一个值。这意味着如果可迭代数据结构是非线性的（例如树），迭代将会使其线性化。



    可迭代的数据结构
        以下是可迭代的值:

        Array
        String
        Map
        Set
        Dom元素
        我们将使用 for...of 循环（参见下文的 for...of 循环）对数据结构进行迭代。
            #Array

            数组 ( Array ) 和类型数组 ( TypedArray ) 他们是可迭代的。

            for (let item of ["zero", "one", "two"]) {
              console.log(item);
            }
            // output:
            // zero
            // one
            // two

            #String

            字符串是可迭代的，单他们遍历的是 Unicode 码，每个码可能包含一个到两个的 Javascript 字符。

            for (const c of 'z\uD83D\uDC0A') {
                console.log(c);
            }
            // output:
            // z
            // \uD83D\uDC0A

            #Map

             Map 主要是迭代它们的 entries ，每个 entry 都会被编码为 [key, value] 的项， entries 是以确定的形势进行迭代，其顺序是与添加的顺序相同。

             const map = new Map();
             map.set(0, "zero");
             map.set(1, "one");

             for (let item of map) {
               console.log(item);
             }
             // output:
             // [0, "zero"]
             // [1, "one"]
             ** 注意： WeakMaps 不可迭代

             #Set

              Set 是对其元素进行迭代，迭代的顺序与其添加的顺序相同

              const set = new Set();
              set.add("zero");
              set.add("one");

              for (let item of set) {
                console.log(item);
              }
              // output:
              // zero
              // one
              注意： WeakSets 不可迭代

              #arguments

               arguments 目前在 ES6 中使用越来越少，但也是可遍历的

               function args() {
                 for (let item of arguments) {
                   console.log(item);
                 }
               }
               args("zero", "one");
               // output:
               // zero
               // one
             #普通对象不可迭代

             // TypeError
             for (let item of {}) {
               console.log(item);
             }

             #for...of循环
             for...of 是 ES6 新引入的循环，用于替代 for..in 和 forEach() ，并且支持新的迭代协议。它可用于迭代常规的数据类型，如 Array 、 String 、 Map 和 Set 等等。


          #迭代常规数据类型
          Array
          String
          Map
          Set

         #可迭代的数据结构
            ##of 操作数必须是可迭代，这意味着如果是普通对象则无法进行迭代。如果数据结构类似于数组的形式，则可以借助 Array.from() 方法进行转换迭代。
            const arrayLink = {length: 2, 0: "zero", 1: "one"}
            // 报 TypeError 异常
            for (let item of arrayLink) {
              console.log(item);
            }

            // 正常运行
            // output:
            // zero
            // one
            for (let item of Array.from(arrayLink)) {
              console.log(item);
            }
           ##let 、const 和 var 用于 for..of
           如果使用 let 和 const ，每次迭代将会创建一个新的存储空间，这可以保证作用域在迭代的内部。
           const nums = ["zero", "one", "two"];

           for (const num of nums) {
             console.log(num);
           }
           // 报 ReferenceError
           console.log(num);
           *** 从上面的例子我们看到，最后一句会报异常，原因 num 的作用域只在循环体内部，外部无效，具体可查阅 let 与 const 章节。使用 var 则不会出现上述情况，因为 var 会作用于全局，迭代将不会每次都创建一个新的存储空间。
            const nums = ["zero", "one", "two"];

            forv (var num of nums) {
              console.log(num);
            }
            // output: two
            console.log(num);
