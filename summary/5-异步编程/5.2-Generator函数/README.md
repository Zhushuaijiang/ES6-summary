#ES6 Generator 函数
    ES6 新引入了 Generator 函数，可以通过 yield 关键字，把函数的执行流挂起，为改变执行流程提供了可能，从而为异步编程提供解决方案。 基本用法

    Generator 函数组成
    Generator 有两个区分于普通函数的部分：

    一是在 function 后面，函数名之前有个 * ；

    函数内部有 yield 表达式。

    其中 * 用来表示函数为 Generator 函数，yield 用来定义函数内部的状态。

    function* func(){
     console.log("one");
     yield '1';
     console.log("two");
     yield '2';
     console.log("three");
     return '3';
    }
    执行机制
    调用 Generator 函数和调用普通函数一样，在函数名后面加上()即可，但是 Generator 函数不会像普通函数一样立即执行，而是返回一个指向内部状态对象的指针，所以要调用遍历器对象Iterator 的 next 方法，指针就会从函数头部或者上一次停下来的地方开始执行。

    f.next();
    // one
    // {value: "1", done: false}

    f.next();
    // two
    // {value: "2", done: false}

    f.next();
    // three
    // {value: "3", done: true}

    f.next();
    // {value: undefined, done: true}
    第一次调用 next 方法时，从 Generator 函数的头部开始执行，先是打印了 one ,执行到 yield 就停下来，并将yield 后边表达式的值 '1'，作为返回对象的 value 属性值，此时函数还没有执行完， 返回对象的 done 属性值是 false。

    第二次调用 next 方法时，同上步 。

    第三次调用 next 方法时，先是打印了 three ，然后执行了函数的返回操作，并将 return 后面的表达式的值，作为返回对象的 value 属性值，此时函数已经结束，多以 done 属性值为true 。

    第四次调用 next 方法时， 此时函数已经执行完了，所以返回 value 属性值是 undefined ，done 属性值是 true 。如果执行第三步时，没有 return 语句的话，就直接返回 {value: undefined, done: true}。

    函数返回的遍历器对象的方法
    next 方法

    一般情况下，next 方法不传入参数的时候，yield 表达式的返回值是 undefined 。当 next 传入参数的时候，该参数会作为上一步yield的返回值。

    function* sendParameter(){
        console.log("strat");
        var x = yield '2';
        console.log("one:" + x);
        var y = yield '3';
        console.log("two:" + y);
        console.log("total:" + (x + y));
    }
    next不传参

    var sendp1 = sendParameter();
    sendp1.next();
    // strat
    // {value: "2", done: false}
    sendp1.next();
    // one:undefined
    // {value: "3", done: false}
    sendp1.next();
    // two:undefined
    // total:NaN
    // {value: undefined, done: true}
    next传参
    var sendp2 = sendParameter();
    sendp2.next(10);
    // strat
    // {value: "2", done: false}
    sendp2.next(20);
    // one:20
    // {value: "3", done: false}
    sendp2.next(30);
    // two:30
    // total:50
    // {value: undefined, done: true}
    除了使用 next ，还可以使用 for... of 循环遍历 Generator 函数生产的 Iterator 对象。

    return 方法

    return 方法返回给定值，并结束遍历 Generator 函数。

    return 方法提供参数时，返回该参数；不提供参数时，返回 undefined 。

    function* foo(){
        yield 1;
        yield 2;
        yield 3;
    }
    var f = foo();
    f.next();
    // {value: 1, done: false}
    f.return("foo");
    // {value: "foo", done: true}
    f.next();
    // {value: undefined, done: true}
    throw 方法
    throw 方法可以再 Generator 函数体外面抛出异常，再函数体内部捕获。
    var g = function* () {
      try {
        yield;
      } catch (e) {
        console.log('catch inner', e);
      }
    };

    var i = g();
    i.next();

    try {
      i.throw('a');
      i.throw('b');
    } catch (e) {
      console.log('catch outside', e);
    }
    // catch inner a
    // catch outside b
    遍历器对象抛出了两个错误，第一个被 Generator 函数内部捕获，第二个因为函数体内部的catch 函数已经执行过了，不会再捕获这个错误，所以这个错误就抛出 Generator 函数体，被函数体外的 catch 捕获。

    yield* 表达式

    yield* 表达式表示 yield 返回一个遍历器对象，用于在 Generator 函数内部，调用另一个 Generator 函数。

    function* callee() {
        console.log('callee: ' + (yield));
    }
    function* caller() {
        while (true) {
            yield* callee();
        }
    }
    const callerObj = caller();
    callerObj.next();
    // {value: undefined, done: false}
    callerObj.next("a");
    // callee: a
    // {value: undefined, done: false}
    callerObj.next("b");
    // callee: b
    // {value: undefined, done: false}

    // 等同于
    function* caller() {
        while (true) {
            for (var value of callee) {
              yield value;
            }
        }
    }
    使用场景
    实现 Iterator

    为不具备 Iterator 接口的对象提供遍历方法。

    function* objectEntries(obj) {
        const propKeys = Reflect.ownKeys(obj);
        for (const propKey of propKeys) {
            yield [propKey, obj[propKey]];
        }
    }

    const jane = { first: 'Jane', last: 'Doe' };
    for (const [key,value] of objectEntries(jane)) {
        console.log(`${key}: ${value}`);
    }
    // first: Jane
    // last: Doe
    Reflect.ownKeys() 返回对象所有的属性，不管属性是否可枚举，包括 Symbol。

    jane 原生是不具备 Iterator 接口无法通过 for... of遍历。这边用了 Generator 函数加上了 Iterator 接口，所以就可以遍历 jane 对象了。