#函数参数的扩展
    默认参数
        function fn(name,age=17){
         console.log(name+","+age);
        }
        fn("Amy",18);  // Amy,18
        fn("Amy","");  // Amy,
        fn("Amy");     // Amy,17
        ** 注意点：使用函数默认参数时，不允许有同名参数。
        // 不报错
        function fn(name,name){
         console.log(name);
        }

        // 报错
        //SyntaxError: Duplicate parameter name not allowed in this context
        function fn(name,name,age=17){
         console.log(name+","+age);
        }
        ** 只有在未传递参数，或者参数为 undefined 时，才会使用默认参数，null 值被认为是有效的值传递。

        函数参数默认值存在暂时性死区，在函数参数默认值表达式中，还未初始化赋值的参数值无法作为其他参数的默认值。
        function f(x,y=x){
            console.log(x,y);
        }
        f(1);  // 1 1

        function f(x=y){
            console.log(x);
        }
        f();  // ReferenceError: y is not defined


        不定参数
            不定参数用来表示不确定参数个数，形如，...变量名，由...加上一个具名参数标识符组成。具名参数只能放在参数组的最后，并且有且只有一个不定参数。

            function f(...values){
                console.log(values.length);
            }
            f(1,2);      //2
            f(1,2,3,4);  //4
         箭头函数提供了一种更加简洁的函数书写方式。基本语法是：
            参数 => 函数体
            var f = v => v;
            //等价于
            var f = function(a){
             return a;
            }
            f(1);  //1
         当箭头函数没有参数或者有多个参数，要用 () 括起来。
            var f = (a,b) => a+b;
            f(6,2);  //8

          当箭头函数函数体有多行语句，用 {} 包裹起来，表示代码块，当只有一行语句，并且需要返回结果时，可以省略 {} , 结果会自动返回。
          var f = (a,b) => {
           let result = a+b;
           return result;
          }
          f(6,2);  // 8

         当箭头函数要返回对象的时候，为了区分于代码块，要用 () 将对象包裹起来
         //报错
         var f = (id,name) => {id: id, name: name};
         f(6,2);  // SyntaxError: Unexpected token :

         // 不报错
         var f = (id,name) => ({id: id, name: name});
         f(6,2);  // {id: 6, name: 2}
            ** 注意点：没有 this、super、arguments 和 new.target 绑定。
                var func = () => {
                  // 箭头函数里面没有 this 对象，
                  // 此时的 this 是外层的 this 对象，即 Window
                  console.log(this)
                }
                func(55)  // Window

                var func = () => {
                  console.log(arguments)
                }
                func(55);  // ReferenceError: arguments is not defined
             箭头函数体中的 this 对象，是定义函数时的对象，而不是使用函数时的对象。
                function fn(){
                  setTimeout(()=>{
                    // 定义时，this 绑定的是 fn 中的 this 对象
                    console.log(this.a);
                  },0)
                }
                var a = 20;
                // fn 的 this 对象为 {a: 19}
                fn.call({a: 18});  // 18
                ** 不可以作为构造函数，也就是不能使用 new 命令，否则会报错

            适合使用的场景

                    ES6 之前，JavaScript 的 this 对象一直很令人头大，回调函数，经常看到 var self = this 这样的代码，为了将外部 this 传递到回调函数中，那么有了箭头函数，就不需要这样做了，直接使用 this 就行。

                    // 回调函数
                    var Person = {
                        'age': 18,
                        'sayHello': function () {
                          setTimeout(function () {
                            console.log(this.age);
                          });
                        }
                    };
                    var age = 20;
                    Person.sayHello();  // 20

                    var Person1 = {
                        'age': 18,
                        'sayHello': function () {
                          setTimeout(()=>{
                            console.log(this.age);
                          });
                        }
                    };
                    var age = 20;
                    Person1.sayHello();  // 18
                        所以，当我们需要维护一个 this 上下文的时候，就可以使用箭头函数。

                    不适合使用的场景
                        定义函数的方法，且该方法中包含 this
                        var Person = {
                            'age': 18,
                            'sayHello': ()=>{
                                console.log(this.age);
                              }
                        };
                        var age = 20;
                        Person.sayHello();  // 20
                        // 此时 this 指向的是全局对象

                        var Person1 = {
                            'age': 18,
                            'sayHello': function () {
                                console.log(this.age);
                            }
                        };
                        var age = 20;
                        Person1.sayHello();   // 18
                        // 此时的 this 指向 Person1 对象

                        需要动态 this 的时候
                       var button = document.getElementById('userClick');
                       button.addEventListener('click', () => {
                            this.classList.toggle('on');
                       });
                       button 的监听函数是箭头函数，所以监听函数里面的 this 指向的是定义的时候外层的 this 对象，即 Window，导致无法操作到被点击的按钮对象。



