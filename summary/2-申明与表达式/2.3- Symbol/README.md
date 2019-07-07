ES6 Symbol
    概述：、
    ES6 引入了一种新的原始数据类型 Symbol ，表示独一无二的值，最大的用法是用来定义对象的唯一属性名。
    ES6 数据类型除了 Number 、 String 、 Boolean 、 Objec t、 null 和 undefined ，还新增了 Symbol 。

  #  基本用法
    Symbol 函数栈不能用 new 命令，因为 Symbol 是原始数据类型，不是对象。
    可以接受一个字符串作为参数，为新创建的 Symbol 提供描述，用来显示在控制台或者作为字符串的时候使用，便于区分。
    let sy = Symbol("KK");
    console.log(sy);   // Symbol(KK)
    typeof(sy);        // "symbol"
    // 相同参数 Symbol() 返回的值不相等
    let sy1 = Symbol("kk");
    sy === sy1;       // false

   # 使用场景
      作为属性名
      由于每一个 Symbol 的值都是不相等的，所以 Symbol 作为对象的属性名，可以保证属性不重名。
      let sy = Symbol("key1");
      // 写法1
      let syObject = {};
      syObject[sy] = "kk";
      console.log(syObject);    // {Symbol(key1): "kk"}
      // 写法2
      let syObject = {
        [sy]: "kk"
      };
      console.log(syObject);    // {Symbol(key1): "kk"}
      // 写法3
      let syObject = {};
      Object.defineProperty(syObject, sy, {value: "kk"});
      console.log(syObject);   // {Symbol(key1): "kk"}
