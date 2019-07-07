#模块化
    概述
    在 ES6 前， 实现模块化使用的是 RequireJS 或者 seaJS（分别是基于 AMD 规范的模块化库，  和基于 CMD 规范的模块化库）。

    ES6 引入了模块化，其设计思想是在编译时就能确定模块的依赖关系，以及输入和输出的变量。

    ES6 的模块化分为导出（export） @与导入（import）两个模块。

    特点
    ES6 的模块自动开启严格模式，不管你有没有在模块头部加上 use strict;。

    模块中可以导入和导出各种类型的变量，如函数，对象，字符串，数字，布尔值，类等。

    每个模块都有自己的上下文，每一个模块内声明的变量都是局部变量，不会污染全局作用域。

    每一个模块只加载一次（是单例的）， 若再去加载同目录下同文件，直接从内存中读取。

    export 与 import
    基本用法
    模块导入导出各种类型的变量，如字符串，数值，函数，类。

    导出的函数声明与类声明必须要有名称（export default 命令另外考虑）。
    不仅能导出声明还能导出引用（例如函数）。
    export 命令可以出现在模块的任何位置，但必需处于模块顶层。
    import 命令会提升到整个模块的头部，首先执行。
    /*-----export [test.js]-----*/
    let myName = "Tom";
    let myAge = 20;
    let myfn = function(){
        return "My name is" + myName + "! I'm '" + myAge + "years old."
    }
    let myClass =  class myClass {
        static a = "yeah!";
    }
    export { myName, myAge, myfn, myClass }

    /*-----import [xxx.js]-----*/
    import { myName, myAge, myfn, myClass } from "./test.js";
    console.log(myfn());// My name is Tom! I'm 20 years old.
    console.log(myAge);// 20
    console.log(myName);// Tom
    console.log(myClass.a );// yeah!
    建议使用大括号指定所要输出的一组变量写在文档尾部，明确导出的接口。

    函数与类都需要有对应的名称，导出文档尾部也避免了无对应名称。

    as 的用法
    export 命令导出的接口名称，须和模块内部的变量有一一对应关系。

    导入的变量名，须和导出的接口名称相同，即顺序可以不一致。

    /*-----export [test.js]-----*/
    let myName = "Tom";
    export { myName as exportName }

    /*-----import [xxx.js]-----*/
    import { exportName } from "./test.js";
    console.log(exportName);// Tom
    使用 as 重新定义导出的接口名称，隐藏模块内部的变量
    /*-----export [test1.js]-----*/
    let myName = "Tom";
    export { myName }
    /*-----export [test2.js]-----*/
    let myName = "Jerry";
    export { myName }
    /*-----import [xxx.js]-----*/
    import { myName as name1 } from "./test1.js";
    import { myName as name2 } from "./test2.js";
    console.log(name1);// Tom
    console.log(name2);// Jerry
    不同模块导出接口名称命名重复， 使用 as 重新定义变量名。

    import 命令的特点
    只读属性：不允许在加载模块的脚本里面，改写接口的引用指向，即可以改写 import 变量类型为对象的属性值，不能改写 import 变量类型为基本类型的值。

    import {a} from "./xxx.js"
    a = {}; // error

    import {a} from "./xxx.js"
    a.foo = "hello"; // a = { foo : 'hello' }
    单例模式：多次重复执行同一句 import 语句，那么只会执行一次，而不会执行多次。import 同一模块，声明不同接口引用，会声明对应变量，但只执行一次 import 。

    import { a } "./xxx.js";
    import { a } "./xxx.js";
    // 相当于 import { a } "./xxx.js";

    import { a } from "./xxx.js";
    import { b } from "./xxx.js";
    // 相当于 import { a, b } from "./xxx.js";
    静态执行特性：import 是静态执行，所以不能使用表达式和变量。

    import { "f" + "oo" } from "methods";
    // error
    let module = "methods";
    import { foo } from module;
    // error
    if (true) {
      import { foo } from "method1";
    } else {
      import { foo } from "method2";
    }
    // error
    export default 命令
    在一个文件或模块中，export、import 可以有多个，export default 仅有一个。
    export default 中的 default 是对应的导出接口变量。
    通过 export 方式导出，在导入时要加{ }，export default 则不需要。
    export default 向外暴露的成员，可以使用任意变量来接收。
    var a = "My name is Tom!";
    export default a; // 仅有一个
    export default var c = "error";
    // error，default 已经是对应的导出变量，不能跟着变量声明语句

    import b from "./xxx.js"; // 不需要加{}， 使用任意变量接收
    复合使用
    注：import() 是提案，这边暂时不延伸讲解。

    export 与 import 可以在同一模块使用，使用特点：

    可以将导出接口改名，包括 default。
    复合使用 export 与 import ，也可以导出全部，当前模块导出的接口会覆盖继承导出的。
    export { foo, bar } from "methods";

    // 约等于下面两段语句，不过上面导入导出方式该模块没有导入 foo 与 bar
    import { foo, bar } from "methods";
    export { foo, bar };

    /* ------- 特点 1 --------*/
    // 普通改名
    export { foo as bar } from "methods";
    // 将 foo 转导成 default
    export { foo as default } from "methods";
    // 将 default 转导成 foo
    export { default as foo } from "methods";

    /* ------- 特点 2 --------*/
    export * from "methods";