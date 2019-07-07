3.2.1
    #ES6 字符串
    ES6之前：ES6 之前判断字符串是否包含子串，用 indexOf 方法，ES6 新增了子串的识别方法。
    ES6之后：includes()：返回布尔值，判断是否找到参数字符串。
            startsWith()：返回布尔值，判断参数字符串是否在原字符串的头部。
            endsWith()：返回布尔值，判断参数字符串是否在原字符串的尾部。
    let string = "apple,banana,orange";
    string.includes("banana");     // true
    string.startsWith("apple");    // true
    string.endsWith("apple");      // false
    string.startsWith("banana",6)  // true
         **注意点：1：这三个方法只返回布尔值，如果需要知道子串的位置，还是得用 indexOf 和 lastIndexOf 。
                   2：这三个方法如果传入了正则表达式而不是字符串，会抛出错误。而 indexOf 和 lastIndexOf 这两个方法，它们会将正则表达式转换为字符串并搜索它。

     #字符串重复
     repeat()：返回新的字符串，表示将字符串重复指定次数返回。
        console.log("Hello,".repeat(2));  // "Hello,Hello,"
     如果参数是小数，向下取整
       console.log("Hello,".repeat(3.2));  // "Hello,Hello,Hello,"
     如果参数是 0 至 -1 之间的小数，会进行取整运算，0 至 -1 之间的小数取整得到 -0 ，等同于 repeat 零次
        console.log("Hello,".repeat(-0.5));  // ""
     如果参数是 NaN，等同于 repeat 零次
        console.log("Hello,".repeat(NaN));  // ""

     如果参数是负数或者 Infinity ，会报错:
        console.log("Hello,".repeat(-1));
        // RangeError: Invalid count value

        console.log("Hello,".repeat(Infinity));
        // RangeError: Invalid count value

    如果传入的参数是字符串，则会先将字符串转化为数字
        console.log("Hello,".repeat("hh")); // ""
        console.log("Hello,".repeat("2"));  // "Hello,Hello,"
    字符串补全
        padStart：返回新的字符串，表示用参数字符串从头部（左侧）补全原字符串。
        padEnd：返回新的字符串，表示用参数字符串从尾部（右侧）补全原字符串。
    以上两个方法接受两个参数，第一个参数是指定生成的字符串的最小长度，第二个参数是用来补全的字符串。如果没有指定第二个参数，默认用空格填充。
        console.log("h".padStart(5,"o"));  // "ooooh"
        console.log("h".padEnd(5,"o"));    // "hoooo"
        console.log("h".padStart(5));      // "    h"
    如果指定的长度小于或者等于原字符串的长度，则返回原字符串:
        console.log("hello".padStart(5,"A"));  // "hello"
    如果原字符串加上补全字符串长度大于指定长度，则截去超出位数的补全字符串:
        console.log("hello".padEnd(10,",world!"));  // "hello,worl"
    常用于补全位数：
        console.log("123".padStart(10,"0"));  // "0000000123"

    #模板字符串
        模板字符串相当于加强版的字符串，用反引号 `,除了作为普通字符串，还可以用来定义多行字符串，还可以在字符串中加入变量和表达式。
        1：普通：
            let string = `Hello'\n'world`;
            console.log(string);
            // "Hello'
            // 'world"
        2：多行：
            let string1 =  `Hey,
            can you stop angry now?`;
            console.log(string1);
            // Hey,
            // can you stop angry now?
         3:字符串插入变量和表达式。
            变量名写在 ${} 中，${} 中可以放入 JavaScript 表达式。
            let name = "Mike";
            let age = 27;
            let info = `My Name is ${name},I am ${age+1} years old next year.`
            console.log(info);
            // My Name is Mike,I am 28 years old next year.
         4:字符串中调用函数：
            function f(){
              return "have fun!";
            }
            let string2= `Game start,${f()}`;
            console.log(string2);  // Game start,have fun!

         ***注意点：模板字符串中的换行和空格都是会被保留的
     #标签模板
        alert`Hello world!`;
        // 等价于
        alert('Hello world!');
        当模板字符串中带有变量，会将模板字符串参数处理成多个参数。
        function f(stringArr,...values){
         let result = "";
         for(let i=0;i<stringArr.length;i++){
          result += stringArr[i];
          if(values[i]){
           result += values[i];
                }
            }
         return result;
        }
        let name = 'Mike';
        let age = 27;
        f`My Name is ${name},I am ${age+1} years old next year.`;
        // "My Name is Mike,I am 28 years old next year."

        f`My Name is ${name},I am ${age+1} years old next year.`;
        // 等价于
        f(['My Name is',',I am ',' years old next year.'],'Mike',28);
    ##应用
    function f(stringArr,...values){
     let result = "";
     for(let i=0;i<stringArr.length;i++){
      result += stringArr[i];
       if(values[i]){
         result += String(values[i]).replace(/&/g, "&amp;")
                   .replace(/</g, "&lt;")
                   .replace(/>/g, "&gt;");
        }
     }
     return result;
    }
    name = '<Amy&MIke>';
    f`<p>Hi, ${name}.I would like send you some message.</p>`;
    // <p>Hi, &lt;Amy&amp;MIke&gt;.I would like send you some message.</p>
    国际化处理（转化多国语言）
    i18n`Hello ${name}, you are visitor number ${visitorNumber}.`;

#3.2.2 ES6 数值
    二进制表示法新写法: 前缀 0b 或 0B 。
    console.log(0b11 === 3); // true
    console.log(0B11 === 3); // true
    八进制表示法新写法: 前缀 0o 或 0O 。
    console.log(0o11 === 9); // true
    console.log(0O11 === 9); // true
    常量
    Number.EPSILON
        Number.EPSILON 属性表示 1 与大于 1 的最小浮点数之间的差。
        它的值接近于 2.2204460492503130808472633361816E-16，或者 2-52。

        测试数值是否在误差范围内:
        0.1 + 0.2 === 0.3; // false
        // 在误差范围内即视为相等
        equal = (Math.abs(0.1 - 0.3 + 0.2) < Number.EPSILON); // true
     属性特性
        writable：false
        enumerable：false
        configurable：false
     最大/最小安全整数
        安全整数
        安全整数表示在 JavaScript 中能够精确表示的整数，安全整数的范围在 2 的 -53 次方到 2 的 53 次方之间（不包括两个端点），超过这个范围的整数无法精确表示。
        #最大安全整数
            安全整数范围的上限，即 2 的 53 次方减 1 。
            Number.MAX_SAFE_INTEGER + 1 === Number.MAX_SAFE_INTEGER + 2; // true
            Number.MAX_SAFE_INTEGER === Number.MAX_SAFE_INTEGER + 1;     // false
            Number.MAX_SAFE_INTEGER - 1 === Number.MAX_SAFE_INTEGER - 2; // false
         #最小安全整数
            安全整数范围的下限，即 2 的 53 次方减 1 的负数。
            Number.MIN_SAFE_INTEGER + 1 === Number.MIN_SAFE_INTEGER + 2; // false
            Number.MIN_SAFE_INTEGER === Number.MIN_SAFE_INTEGER - 1;     // false
            Number.MIN_SAFE_INTEGER - 1 === Number.MIN_SAFE_INTEGER - 2; // true
     方法
     Number 对象新方法
            Number.isFinite()
            用于检查一个数值是否为有限的（ finite ），即不是 Infinity
            console.log( Number.isFinite(1));   // true
            console.log( Number.isFinite(0.1)); // true

            // NaN 不是有限的
            console.log( Number.isFinite(NaN)); // false

            console.log( Number.isFinite(Infinity));  // false
            console.log( Number.isFinite(-Infinity)); // false

            // Number.isFinate 没有隐式的 Number() 类型转换，所有非数值都返回 false
            console.log( Number.isFinite('foo')); // false
            console.log( Number.isFinite('15'));  // false
            console.log( Number.isFinite(true));  // false
            Number.isNaN()
            用于检查一个值是否为 NaN 。
            console.log(Number.isNaN(NaN));      // true
            console.log(Number.isNaN('true'/0)); // true

            // 在全局的 isNaN() 中，以下皆返回 true，因为在判断前会将非数值向数值转换
            // 而 Number.isNaN() 不存在隐式的 Number() 类型转换，非 NaN 全部返回 false
            Number.isNaN("NaN");      // false
            Number.isNaN(undefined);  // false
            Number.isNaN({});         // false
            Number.isNaN("true");     // false

           从全局移植到 Number 对象的方法
           逐步减少全局方法，用于全局变量的模块化。
           方法的行为没有发生改变。

            Number.parseInt()

            用于将给定字符串转化为指定进制的整数。
            // 不指定进制时默认为 10 进制
            Number.parseInt('12.34'); // 12
            Number.parseInt(12.34);   // 12

            // 指定进制
            Number.parseInt('0011',2); // 3

            // 与全局的 parseInt() 函数是同一个函数
            Number.parseInt === parseInt; // true
            Number.parseFloat()
            用于把一个字符串解析成浮点数。
            Number.parseFloat('123.45')    // 123.45
            Number.parseFloat('123.45abc') // 123.45

            // 无法被解析成浮点数，则返回 NaN
            Number.parseFloat('abc') // NaN

            // 与全局的 parseFloat() 方法是同一个方法
            Number.parseFloat === parseFloat // true
            Number.isInteger()
            用于判断给定的参数是否为整数。
            Number.isInteger(value)
            Number.isInteger(0); // true
            // JavaScript 内部，整数和浮点数采用的是同样的储存方法,因此 1 与 1.0 被视为相同的值
            Number.isInteger(1);   // true
            Number.isInteger(1.0); // true

            Number.isInteger(1.1);     // false
            Number.isInteger(Math.PI); // false

            // NaN 和正负 Infinity 不是整数
            Number.isInteger(NaN);       // false
            Number.isInteger(Infinity);  // false
            Number.isInteger(-Infinity); // false

            Number.isInteger("10");  // false
            Number.isInteger(true);  // false
            Number.isInteger(false); // false
            Number.isInteger([1]);   // false

            // 数值的精度超过 53 个二进制位时，由于第 54 位及后面的位被丢弃，会产生误判
            Number.isInteger(1.0000000000000001) // true

            // 一个数值的绝对值小于 Number.MIN_VALUE（5E-324），即小于 JavaScript 能够分辨
            // 的最小值，会被自动转为 0，也会产生误判
            Number.isInteger(5E-324); // false
            Number.isInteger(5E-325); // true
            Number.isSafeInteger()
            用于判断数值是否在安全范围内。
            Number.isSafeInteger(Number.MIN_SAFE_INTEGER - 1); // false
            Number.isSafeInteger(Number.MAX_SAFE_INTEGER + 1); // false
    ##Math 对象的扩展(ES6 在 Math 对象上新增了 17 个数学相关的静态方法，这些方法只能在 Math 中调用。)
      #普通计算:Math.cbrt (用于计算一个数的立方根。)
        Math.cbrt(1);  // 1
        Math.cbrt(0);  // 0
        Math.cbrt(-1); // -1
        // 会对非数值进行转换
        Math.cbrt('1'); // 1

        // 非数值且无法转换为数值时返回 NaN
        Math.cbrt('hhh'); // NaN

        Math.imul（两个数以 32 位带符号整数形式相乘的结果，返回的也是一个 32 位的带符号整数。）
        // 大多数情况下，结果与 a * b 相同
        Math.imul(1, 2);   // 2
        // 用于正确返回大数乘法结果中的低位数值
        Math.imul(0x7fffffff, 0x7fffffff); // 1
        //Math.hypot
        Math.hypot(3, 4); // 5

        // 非数值会先被转换为数值后进行计算
        Math.hypot(1, 2, '3'); // 3.741657386773941
        Math.hypot(true);      // 1
        Math.hypot(false);     // 0

        // 空值会被转换为 0
        Math.hypot();   // 0
        Math.hypot([]); // 0

        // 参数为 Infinity 或 -Infinity 返回 Infinity
        Math.hypot(Infinity); // Infinity
        Math.hypot(-Infinity); // Infinity

        // 参数中存在无法转换为数值的参数时返回 NaN
        Math.hypot(NaN);         // NaN
        Math.hypot(3, 4, 'foo'); // NaN
        Math.hypot({});          // NaN

        Math.clz32
        Math.clz32(0); // 32
        Math.clz32(1); // 31
        Math.clz32(0b01000000000100000000000000000000); // 1

        // 当参数为小数时，只考虑整数部分
        Math.clz32(0.5); // 32

        // 对于空值或非数值，会转化为数值再进行计算
        Math.clz32('1');       // 31
        Math.clz32();          // 32
        Math.clz32([]);        // 32
        Math.clz32({});        // 32
        Math.clz32(NaN);       // 32
        Math.clz32(Infinity);  // 32
        Math.clz32(-Infinity); // 32
        Math.clz32(undefined); // 32
        Math.clz32('hhh');     // 32

        Math.trunc
        用于返回数字的整数部分。
        Math.trunc(12.3); // 12
        Math.trunc(12);   // 12

        // 整数部分为 0 时也会判断符号
        Math.trunc(-0.5); // -0
        Math.trunc(0.5);  // 0

        // Math.trunc 会将非数值转为数值再进行处理
        Math.trunc("12.3"); // 12

        // 空值或无法转化为数值时时返回 NaN
        Math.trunc();           // NaN
        Math.trunc(NaN);        // NaN
        Math.trunc("hhh");      // NaN
        Math.trunc("123.2hhh"); // NaN

        Math.fround
            // 对于 2 的 24 次方取负至 2 的 24 次方之间的整数（不含两个端点），返回结果与参数本身一致
            Math.fround(-(2**24)+1);  // -16777215
            Math.fround(2 ** 24 - 1); // 16777215

            / 用于将 64 位双精度浮点数转为 32 位单精度浮点数
            Math.fround(1.234) // 1.125
            // 当小数的精度超过 24 个二进制位，会丢失精度
            Math.fround(0.3); // 0.30000001192092896
            // 参数为 NaN 或 Infinity 时返回本身
            Math.fround(NaN)      // NaN
            Math.fround(Infinity) // Infinity

            // 参数为其他非数值类型时会将参数进行转换
            Math.fround('5');  // 5
            Math.fround(true); // 1
            Math.fround(null); // 0
            Math.fround([]);   // 0
            Math.fround({});   // NaN
       Math.sign(判断数字的符号（正、负、0）。)
            Math.sign(1);  // 1
            Math.sign(-1); // -1

            // 参数为 0 时，不同符号的返回不同
            Math.sign(0);  // 0
            Math.sign(-0); // -0

            // 判断前会对非数值进行转换
            Math.sign('1');  // 1
            Math.sign('-1'); // -1

            // 参数为非数值（无法转换为数值）时返回 NaN
            Math.sign(NaN);   // NaN
            Math.sign('hhh'); // NaN
       Math.expm1(对数方法)
            Math.expm1(1);  // 1.718281828459045
            Math.expm1(0);  // 0
            Math.expm1(-1); // -0.6321205588285577
            // 会对非数值进行转换
            Math.expm1('0'); //0

            // 参数不为数值且无法转换为数值时返回 NaN
            Math.expm1(NaN); // NaN
       Math.log1p(x)  （用于计算1 + x 的自然对数，即 Math.log(1 + x) 。）
            Math.log1p(1);  // 0.6931471805599453
            Math.log1p(0);  // 0
            Math.log1p(-1); // -Infinity

            // 参数小于 -1 时返回 NaN
            Math.log1p(-2); // NaN
       Math.log10(x)  (用于计算以 10 为底的 x 的对数。)
            Math.log10(1);   // 0
            // 计算前对非数值进行转换
            Math.log10('1'); // 0
            // 参数为0时返回 -Infinity
            Math.log10(0);   // -Infinity
            // 参数小于0或参数不为数值（且无法转换为数值）时返回 NaN
            Math.log10(-1);  // NaN
       Math.log2()  用于计算 2 为底的 x 的对数。
            Math.log2(1);   // 0
            // 计算前对非数值进行转换
            Math.log2('1'); // 0
            // 参数为0时返回 -Infinity
            Math.log2(0);   // -Infinity
            // 参数小于0或参数不为数值（且无法转换为数值）时返回 NaN
            Math.log2(-1);  // NaN

       双曲函数方法
           Math.sinh(x): 用于计算双曲正弦。
           Math.cosh(x): 用于计算双曲余弦。
           Math.tanh(x): 用于计算双曲正切。
           Math.asinh(x): 用于计算反双曲正弦。
           Math.acosh(x): 用于计算反双曲余弦。
           Math.atanh(x): 用于计算反双曲正切

       指数运算符
            1 ** 2; // 1
            // 右结合，从右至左计算
            2 ** 2 ** 3; // 256
            // **=
            let exam = 2;
            exam ** = 2; // 4

3.2.3 ES6 对象
        对象字面量
        ES6允许对象的属性直接写变量，这时候属性名是变量名，属性值是变量值。
        const age = 12;
        const name = "Amy";
        const person = {age, name};
        person   //{age: 12, name: "Amy"}
        //等同于
        const person = {age: age, name: name}

        方法名也可以简写
          const person = {
            sayHi(){
              console.log("Hi");
            }
          }
          person.sayHi();  //"Hi"
          //等同于
          const person = {
            sayHi:function(){
              console.log("Hi");
            }
          }
          person.sayHi();//"Hi"

         如果是Generator 函数，则要在前面加一个星号:
            const obj = {
              * myGenerator() {
                yield 'hello world';
              }
            };
            //等同于
            const obj = {
              myGenerator: function* () {
                yield 'hello world';
              }
            };

          属性名表达式
            const obj = {
             ["he"+"llo"](){
               return "Hi";
              }
            }
            obj.hello();  //"Hi"
            注意点：属性的简洁表示法和属性名表达式不能同时使用，否则会报错。
            const hello = "Hello";
            const obj = {
             [hello]
            };
            obj  //SyntaxError: Unexpected token }

            const hello = "Hello";
            const obj = {
             [hello+"2"]:"world"
            };
            obj  //{Hello2: "world"}
          对象的拓展运算符
            拓展运算符（...）用于取出参数对象所有可遍历属性然后拷贝到当前对象。

            let person = {name: "Amy", age: 15};
            let someone = { ...person };
            someone;  //{name: "Amy", age: 15}

           可用于合并两个对象
            let age = {age: 15};
            let name = {name: "Amy"};
            let person = {...age, ...name};
            person;  //{age: 15, name: "Amy"}
                注意点:自定义的属性和拓展运算符对象里面属性的相同的时候：自定义的属性在拓展运算符后面，则拓展运算符对象内部同名的属性将被覆盖掉。
                        let person = {name: "Amy", age: 15};
                        let someone = { ...person, name: "Mike", age: 17};
                        someone;  //{name: "Mike", age: 17}
           自定义的属性在拓展运算度前面，则变成设置新对象默认属性值。

            let person = {name: "Amy", age: 15};
            let someone = {name: "Mike", age: 17, ...person};
            someone;  //{name: "Amy", age: 15}

            拓展运算符后面是空对象，没有任何效果也不会报错。
             let a = {...{}, a: 1, b: 2};
             a;  //{a: 1, b: 2}

            拓展运算符后面是null或者undefined，没有效果也不会报错。
               let b = {...null, ...undefined, a: 1, b: 2};
               b;  //{a: 1, b: 2}

            对象的新方法
                Object.assign(target, source_1, ···)
                用于将源对象的所有可枚举属性复制到目标对象中。
                let target = {a: 1};
                let object2 = {b: 2};
                let object3 = {c: 3};
                Object.assign(target,object2,object3);
                // 第一个参数是目标对象，后面的参数是源对象
                target;  // {a: 1, b: 2, c: 3}
                    如果目标对象和源对象有同名属性，或者多个源对象有同名属性，则后面的属性会覆盖前面的属性。
                    如果该函数只有一个参数，当参数为对象时，直接返回该对象；当参数不是对象时，会先将参数转为对象然后返回。
                    Object.assign(3);         // Number {3}
                    typeof Object.assign(3);  // "object"
                因为 null 和 undefined 不能转化为对象，所以会报错:
                    Object.assign(null);       // TypeError: Cannot convert undefined or null to object
                    Object.assign(undefined);  // TypeError: Cannot convert undefined or null to object
                    当参数不止一个时，null 和 undefined 不放第一个，即不为目标对象时，会跳过 null 和 undefined ，不报错
                    Object.assign(1,undefined);  // Number {1}
                    Object.assign({a: 1},null);  // {a: 1}

                    Object.assign(undefined,{a: 1});  // TypeError: Cannot convert undefined or null to object

                    ** assign 的属性拷贝是浅拷贝:
                    let sourceObj = { a: { b: 1}};
                    let targetObj = {c: 3};
                    Object.assign(targetObj, sourceObj);
                    targetObj.a.b = 2;
                    sourceObj.a.b;  // 2
                        同名属性替换
                        targetObj = { a: { b: 1, c:2}};
                        sourceObj = { a: { b: "hh"}};
                        Object.assign(targetObj, sourceObj);
                        targetObj;  // {a: {b: "hh"}}

                     数组的处理
                        Object.assign([2,3], [5]);  // [5,3]
                        会将数组处理成对象，所以先将 [2,3] 转为 {0:2,1:3} ，然后再进行属性复制，所以源对象的 0 号属性覆盖了目标对象的 0。

                Object.is(value1, value2)
                    用来比较两个值是否严格相等，与（===）基本类似。
                    Object.is("q","q");      // true
                    Object.is(1,1);          // true
                    Object.is([1],[1]);      // false
                    Object.is({q:1},{q:1});  // false
                    与（===）的区别
                    //一是+0不等于-0
                    Object.is(+0,-0);  //false
                    +0 === -0  //true
                    //二是NaN等于本身
                    Object.is(NaN,NaN); //true
                    NaN === NaN  //false
3.2.4 ES6 数组
            数组创建
            Array.of()
            console.log(Array.of(1, 2, 3, 4)); // [1, 2, 3, 4]
            // 参数值可为不同类型
            console.log(Array.of(1, '2', true)); // [1, '2', true]
            // 参数为空时返回空数组
            console.log(Array.of()); // []

            Array.from()
            将类数组对象或可迭代对象转化为数组。
            // 参数为数组,返回与原数组一样的数组
            console.log(Array.from([1, 2])); // [1, 2]
            // 参数含空位
            console.log(Array.from([1, , 3])); // [1, undefined, 3]

            Array.from(arrayLike[, mapFn[, thisArg]])
            返回值为转换后的数组。

            arrayLike

            想要转换的类数组对象或可迭代对象。

            console.log(Array.from([1, 2, 3])); // [1, 2, 3]
            mapFn

            可选，map 函数，用于对每个元素进行处理，放入数组的是处理后的元素。

            console.log(Array.from([1, 2, 3], (n) => n * 2)); // [2, 4, 6]

            thisArg

            可选，用于指定 map 函数执行时的 this 对象。

            let map = {
                do: function(n) {
                    return n * 2;
                }
            }
            let arrayLike = [1, 2, 3];
            console.log(Array.from(arrayLike, function (n){
                return this.do(n);
            }, map)); // [2, 4, 6]

            ##类数组对象
                一个类数组对象必须含有 length 属性，且元素属性名必须是数值或者可转换为数值的字符。
                    let arr = Array.from({
                      0: '1',
                      1: '2',
                      2: 3,
                      length: 3
                    });
                    console.log(arr); // ['1', '2', 3]

                    // 没有 length 属性,则返回空数组
                    let array = Array.from({
                      0: '1',
                      1: '2',
                      2: 3,
                    });
                    console.log(array); // []

                    // 元素属性名不为数值且无法转换为数值，返回长度为 length 元素值为 undefined 的数组
                    let array1 = Array.from({
                      a: 1,
                      b: 2,
                      length: 2
                    });
                    console.log(array1); // [undefined, undefined]
             #转换可迭代对象
                ##转换 map
                let map = new Map();
                map.set('key0', 'value0');
                map.set('key1', 'value1');
                console.log(Array.from(map)); // [['key0', 'value0'],['key1',
                // 'value1']]
                ##转换 set
                let arr = [1, 2, 3];
                let set = new Set(arr);
                console.log(Array.from(set)); // [1, 2, 3]
                ##转换字符串
                let str = 'abc';
                console.log(Array.from(str)); // ["a", "b", "c"]

        ###扩展的方法
            #find()
            let arr = Array.of(1, 2, 3, 4);
            console.log(arr.find(item => item > 2)); // 3

            // 数组空位处理为 undefined
            console.log([, 1].find(n => true)); // undefined

            #findIndex()
            查找数组中符合条件的元素索引，若有多个符合条件的元素，则返回第一个元素索引
            let arr = Array.of(1, 2, 1, 3);
            // 参数1：回调函数
            // 参数2(可选)：指定回调函数中的 this 值
            console.log(arr.findIndex(item => item = 1)); // 0

            // 数组空位处理为 undefined
            console.log([, 1].findIndex(n => true)); //0

            #fill()
            let arr = Array.of(1, 2, 3, 4);
            // 参数1：用来填充的值
            // 参数2：被填充的起始索引
            // 参数3(可选)：被填充的结束索引，默认为数组末尾
            console.log(arr.fill(0,1,2)); // [1, 0, 3, 4]

            #copyWithin()
            将一定范围索引的数组元素修改为此数组另一指定范围索引的元素。
            // 参数1：被修改的起始索引
            // 参数2：被用来覆盖的数据的起始索引
            // 参数3(可选)：被用来覆盖的数据的结束索引，默认为数组末尾
            console.log([1, 2, 3, 4].copyWithin(0,2,4)); // [3, 4, 3, 4]

            // 参数1为负数表示倒数
            console.log([1, 2, 3, 4].copyWithin(-2, 0)); // [1, 2, 1, 2]

            console.log([1, 2, ,4].copyWithin(0, 2, 4)); // [, 4, , 4]

            #entries()
                for(let [key, value] of ['a', 'b'].entries()){
                    console.log(key, value);
                }
                // 0 "a"
                // 1 "b"

                // 不使用 for... of 循环
                let entries = ['a', 'b'].entries();
                console.log(entries.next().value); // [0, "a"]
                console.log(entries.next().value); // [1, "b"]

                // 数组含空位
                console.log([...[,'a'].entries()]); // [[0, undefined], [1, "a"]]
             #keys()
                for(let key of ['a', 'b'].keys()){
                    console.log(key);
                }
                // 0
                // 1

                // 数组含空位
                console.log([...[,'a'].keys()]); // [0, 1]

              #values()
                for(let value of ['a', 'b'].values()){
                    console.log(value);
                }
                // "a"
                // "b"

                // 数组含空位
                console.log([...[,'a'].values()]); // [undefined, "a"]

             #includes()
             数组是否包含指定值。
             注意：与 Set 和 Map 的 has 方法区分；Set 的 has 方法用于查找值；Map 的 has 方法用于查找键名。
                // 参数1：包含的指定值
                [1, 2, 3].includes(1);    // true

                // 参数2：可选，搜索的起始索引，默认为0
                [1, 2, 3].includes(1, 2); // false

                // NaN 的包含判断
                [1, NaN, 3].includes(NaN); // true
             #flat()  嵌套数组转一维数组
             console.log([1 ,[2, 3]].flat()); // [1, 2, 3]

             // 指定转换的嵌套层数
             console.log([1, [2, [3, [4, 5]]]].flat(2)); // [1, 2, 3, [4, 5]]

             // 不管嵌套多少层
             console.log([1, [2, [3, [4, 5]]]].flat(Infinity)); // [1, 2, 3, 4, 5]

             // 自动跳过空位
             console.log([1, [2, , 3]].flat());<p> // [1, 2, 3]

             #flatMap()
                先对数组中每个元素进行了的处理，再对数组执行 flat() 方法。
                // 参数1：遍历函数，该遍历函数可接受3个参数：当前元素、当前元素索引、原数组
                // 参数2：指定遍历函数中 this 的指向
                console.log([1, 2, 3].flatMap(n => [n * 2])); // [2, 4, 6]

        数组缓冲区
            数组缓冲区是内存中的一段地址。

            定型数组的基础。

            实际字节数在创建时确定，之后只可修改其中的数据，不可修改大小。

            创建数组缓冲区
                let buffer = new ArrayBuffer(10);
                console.log(buffer.byteLength); // 10
                分割已有数组缓冲区
                let buffer = new ArrayBuffer(10);
                let buffer1 = buffer.slice(1, 3);
                console.log(buffer1.byteLength); // 2
             视图
                视图是用来操作内存的接口。
                视图可以操作数组缓冲区或缓冲区字节的子集,并按照其中一种数值数据类型来读取和写入数据。
                DataView 类型是一种通用的数组缓冲区视图,其支持所有8种数值型数据类型。

                // 默认 DataView 可操作数组缓冲区全部内容
                let buffer = new ArrayBuffer(10);
                    dataView = new DataView(buffer);
                dataView.setInt8(0,1);
                console.log(dataView.getInt8(0)); // 1

                // 通过设定偏移量(参数2)与长度(参数3)指定 DataView 可操作的字节范围
                let buffer1 = new ArrayBuffer(10);
                    dataView1 = new DataView(buffer1, 0, 3);
                dataView1.setInt8(5,1); // RangeError
             定型数组
                数组缓冲区的特定类型的视图。

                可以强制使用特定的数据类型，而不是使用通用的 DataView 对象来操作数组缓冲区。

              创建(通过数组缓冲区生成)
                let buffer = new ArrayBuffer(10),
                    view = new Int8Array(buffer);
                console.log(view.byteLength); // 10

             通过构造函数
                let view = new Int32Array(10);
                console.log(view.byteLength); // 40
                console.log(view.length);     // 10

                // 不传参则默认长度为0
                // 在这种情况下数组缓冲区分配不到空间，创建的定型数组不能用来保存数据
                let view1 = new Int32Array();
                console.log(view1.byteLength); // 0
                console.log(view1.length);     // 0

                // 可接受参数包括定型数组、可迭代对象、数组、类数组对象
                let arr = Array.from({
                  0: '1',
                  1: '2',
                  2: 3,
                  length: 3
                });
                let view2 = new Int16Array([1, 2]),
                    view3 = new Int32Array(view2),
                    view4 = new Int16Array(new Set([1, 2, 3])),
                    view5 = new Int16Array([1, 2, 3]),
                    view6 = new Int16Array(arr);
                console.log(view2 .buffer === view3.buffer); // false
                console.log(view4.byteLength); // 6
                console.log(view5.byteLength); // 6
                console.log(view6.byteLength); // 6
                ** length 属性不可写，如果尝试修改这个值，在非严格模式下会直接忽略该操作，在严格模式下会抛出错误。
                let view = new Int16Array([1, 2]);
                view.length = 3;
                console.log(view.length); // 2
            定型数组可使用 entries()、keys()、values()进行迭代。
                let view = new Int16Array([1, 2]);
                for(let [k, v] of view.entries()){
                    console.log(k, v);
                }
                // 0 1
                // 1 2
             find() 等方法也可用于定型数组，但是定型数组中的方法会额外检查数值类型是否安全,也会通过 Symbol.species 确认方法的返回值是定型数组而非普通数组。concat() 方法由于两个定型数组合并结果不确定，故不能用于定型数组；另外，由于定型数组的尺寸不可更改,可以改变数组的尺寸的方法，例如 splice() ，不适用于定型数组。
                let view = new Int16Array([1, 2]);
                view.find((n) > 1); // 2
             所有定型数组都含有静态 of() 方法和 from() 方法,运行效果分别与 Array.of() 方法和 Array.from() 方法相似,区别是定型数组的方法返回定型数组,而普通数组的方法返回普通数组。
                let view = Int16Array.of(1, 2);
                console.log(view instanceof Int16Array); // true

             定型数组不是普通数组，不继承自 Array 。
                let view = new Int16Array([1, 2]);
                console.log(Array.isArray(view)); // false

             定型数组中增加了 set() 与 subarray() 方法。 set() 方法用于将其他数组复制到已有定型数组, subarray() 用于提取已有定型数组的一部分形成新的定型数组。
                // set 方法
                // 参数1：一个定型数组或普通数组
                // 参数2：可选，偏移量，开始插入数据的位置，默认为0
                let view= new Int16Array(4);
                view.set([1, 2]);
                view.set([3, 4], 2);
                console.log(view); // [1, 2, 3, 4]

                // subarray 方法
                // 参数1：可选，开始位置
                // 参数2：可选，结束位置(不包含结束位置)
                let view= new Int16Array([1, 2, 3, 4]),
                    subview1 = view.subarray(),
                    subview2 = view.subarray(1),
                    subview3 = view.subarray(1, 3);
                console.log(subview1); // [1, 2, 3, 4]
                console.log(subview2); // [2, 3, 4]
                console.log(subview3); // [2, 3]

             #扩展运算符
                复制数组
                let arr = [1, 2],
                    arr1 = [...arr];
                console.log(arr1); // [1, 2]

                // 数组含空位
                let arr2 = [1, , 3],
                    arr3 = [...arr2];
                console.log(arr3); [1, undefined, 3]
                合并数组
                console.log([...[1, 2],...[3, 4]]); // [1, 2, 3, 4]












