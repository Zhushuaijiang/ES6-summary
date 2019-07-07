#let和const

    #知识点
        let
        const

        # let 声明的变量只在 let 命令所在的代码块内有效。
        # const 声明一个只读的常量，一旦声明，常量的值就不能改变。
    ###实战演习##
    打开Terminal控制台 输入：“Node”命令

    ##代码块内有效
    {
      let a = 0;
      a   // 0
    }
    a 报错 ReferenceError: a is not defined

    {
      let a = 0;
      var b = 1;
    }
    a  // ReferenceError: a is not defined
    b  // 1

    ##不能重复声明
    let a = 1;
    let a = 2;
    var b = 3;
    var b = 4;
    a  // Identifier 'a' has already been declared
    b  // 4

    #for 循环计数器很适合用 let
    for (var i = 0; i < 10; i++) {
      setTimeout(function(){
        console.log(i);
      })
    }
    // 输出十个 10
    for (let j = 0; j < 10; j++) {
      setTimeout(function(){
        console.log(j);
      })
    }
    // 输出 0123456789

    ##不存在变量提升
    let 不存在变量提升，var 会变量提升:
    console.log(a);  //ReferenceError: a is not defined
    let a = "apple";
    console.log(b);  //undefined
    var b = "banana";

    #const 命令
    const 声明一个只读变量，声明之后不允许改变(内存地址不变)。意味着，一旦声明必须初始化，否则会报错。
        基本用法:
            const PI = "3.1415926";
            PI  // 3.1415926

            const MY_AGE;  // SyntaxError: Missing initializer in const declaration
        暂时性死区:
            var PI = "a";
            if(true){
              console.log(PI);  // ReferenceError: PI is not defined
              const PI = "3.1415926";
            }



