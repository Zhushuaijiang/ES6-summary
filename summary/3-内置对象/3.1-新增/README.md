##3.1.1
    #Map 对象
    Map 对象保存键值对。任何值(对象或者原始值) 都可以作为一个键或一个值。

    Maps 和 Objects 的区别
    一个 Object 的键只能是字符串或者 Symbols，但一个 Map 的键可以是任意值。
    Map 中的键值是有序的（FIFO 原则），而添加到对象中的键则不是。
    Map 的键值对个数可以从 size 属性获取，而 Object 的键值对个数只能手动计算。
    Object 都有自己的原型，原型链上的键名有可能和你自己在对象上的设置的键名产生冲突。

    #Map 中的 key
        1:key是字符串
            var myMap = new Map();
            var keyString = "a string";

            myMap.set(keyString, "和键'a string'关联的值");

            myMap.get(keyString);    // "和键'a string'关联的值"
            myMap.get("a string");   // "和键'a string'关联的值"
                                     // 因为 keyString === 'a string'
        2： 对象
            var myMap = new Map();
            var keyObj = {},

            myMap.set(keyObj, "和键 keyObj 关联的值");
            ﻿
            myMap.get(keyObj); // "和键 keyObj 关联的值"
            myMap.get({}); // undefined, 因为 keyObj !== {}

        3：key 是函数
            var myMap = new Map();
            var keyFunc = function () {}, // 函数

            myMap.set(keyFunc, "和键 keyFunc 关联的值");

            myMap.get(keyFunc); // "和键 keyFunc 关联的值"
            myMap.get(function() {}) // undefined, 因为 keyFunc !== function () {}

        4：key 是 NaN

            var myMap = new Map();
            myMap.set(NaN, "not a number");
            myMap.get(NaN); // "not a number"
            var otherNaN = Number("foo");
            myMap.get(otherNaN); // "not a number"

    #Map 的迭代
        1：for...of
        2：forEach()

    #Map 对象的操作
        1：Map 与 Array的转换
            var kvArray = [["key1", "value1"], ["key2", "value2"]];
            // Map 构造函数可以将一个 二维 键值对数组转换成一个 Map 对象
            var myMap = new Map(kvArray);
            // 使用 Array.from 函数可以将一个 Map 对象转换成一个二维键值对数组
            var outArray = Array.from(myMap);
        2：Map 的克隆
            var myMap1 = new Map([["key1", "value1"], ["key2", "value2"]]);
            var myMap2 = new Map(myMap1);
            console.log(original === clone);
            // 打印 false。 Map 对象构造函数生成实例，迭代出新的对象。
        3：Map 的合并
            var first = new Map([[1, 'one'], [2, 'two'], [3, 'three'],]);
            var second = new Map([[1, 'uno'], [2, 'dos']]);
            // 合并两个 Map 对象时，如果有重复的键值，则后面的会覆盖前面的，对应值即 uno，dos， three
            var merged = new Map([...first, ...second]);
    #Set 对象
        Set 对象允许你存储任何类型的唯一值，无论是原始值或者是对象引用。
            1：+0 与 -0 在存储判断唯一性的时候是恒等的，所以不重复；
            2：undefined 与 undefined 是恒等的，所以不重复；
            3：NaN 与 NaN 是不恒等的，但是在 Set 中只能存一个，不重复。
        数组去重
        var mySet = new Set([1, 2, 3, 4, 4]);
        [...mySet]; // [1, 2, 3, 4]
        并集
        var a = new Set([1, 2, 3]);
        var b = new Set([4, 3, 2]);
        var union = new Set([...a, ...b]); // {1, 2, 3, 4}
        交集
        var a = new Set([1, 2, 3]);
        var b = new Set([4, 3, 2]);
        var intersect = new Set([...a].filter(x => b.has(x))); // {2, 3}
        差集
        var a = new Set([1, 2, 3]);
        var b = new Set([4, 3, 2]);
        var difference = new Set([...a].filter(x => !b.has(x))); // {1}

 ##3.1.2

        #ES6 Reflect 与 Proxy
            概述：
                #Proxy 与 Reflect 是 ES6 为了操作对象引入的 API 。
                #Proxy 可以对目标对象的读取、函数调用等操作进行拦截，然后进行操作处理。
                它不直接操作对象，而是像代理模式，通过对象的代理对象进行操作，在进行这些操作时，可以添加一些需要的额外操作。
                #Reflect 可以用于获取目标对象的行为，它与 Object 类似，但是更易读，
                为操作对象提供了一种更优雅的方式。它的方法与 Proxy 是对应的。
            基本用法:
                Proxy:
                    let target = {
                        name: 'Tom',
                        age: 24
                    }
                    let handler = {
                        get: function(target, key) {
                            console.log('getting '+key);
                            return target[key]; // 不是target.key
                        },
                        set: function(target, key, value) {
                            console.log('setting '+key);
                            target[key] = value;
                        }
                    }
                    let proxy = new Proxy(target, handler)
                    proxy.name     // 实际执行 handler.get
                    proxy.age = 25 // 实际执行 handler.set
                    // getting name
                    // setting age
                    // 25

                 实例方法
                    get(target, propKey, receiver)
                 let exam ={
                     name: "Tom",
                     age: 24
                 }
                 let proxy = new Proxy(exam, {
                   get(target, propKey, receiver) {
                     console.log('Getting ' + propKey);
                     return target[propKey];
                   }
                 })
                 get() 方法可以继承。
                 let proxy = new Proxy({}, {
                   get(target, propKey, receiver) {
                       // 实现私有属性读取保护
                       if(propKey[0] === '_'){
                           throw new Erro(`Invalid attempt to get private     "${propKey}"`);
                       }
                       console.log('Getting ' + propKey);
                       return target[propKey];
                   }
                 });
                 let obj = Object.create(proxy);
                 obj.name


                 #set(target, propKey, value, receiver)
                 用于拦截 target 对象上的 propKey 的赋值操作。如果目标对象自身的某个属性，不可写且不可配置，那么set方法将不起作用。
                 let validator = {
                     set: function(obj, prop, value) {
                         if (prop === 'age') {
                             if (!Number.isInteger(value)) {
                                 throw new TypeError('The age is not an integer');
                             }
                             if (value > 200) {
                                 throw new RangeError('The age seems invalid');
                             }
                         }
                         // 对于满足条件的 age 属性以及其他属性，直接保存
                         obj[prop] = value;
                     }
                 };
                 let proxy= new Proxy({}, validator)
                 proxy.age = 100;
                 proxy.age           // 100
                 proxy.age = 'oppps' // 报错 TypeError: The age is not an integer at Object.set
                 proxy.age = 300     // 报错  RangeError: The age seems invalid

                 第四个参数 receiver 表示原始操作行为所在对象，一般是 Proxy 实例本身。
                 const handler = {
                     set: function(obj, prop, value, receiver) {
                         obj[prop] = receiver;
                     }
                 };
                 const proxy = new Proxy({}, handler);
                 proxy.name= 'Tom';
                 proxy.name=== proxy // true

                 const exam = {}
                 Object.setPrototypeOf(exam, proxy)
                 exam.name = "Tom"
                 exam.name === exam // true
                 注意，严格模式下，set代理如果没有返回true，就会报错。

             #apply(target, ctx, args)
                用于拦截函数的调用、call 和 reply 操作。target 表示目标对象，ctx 表示目标对象上下文，args 表示目标对象的参数数组。
                function sub(a, b){
                    return a - b;
                }
                let handler = {
                    apply: function(target, ctx, args){
                        console.log('handle apply');
                        return Reflect.apply(...arguments);
                    }
                }
                let proxy = new Proxy(sub, handler)
                proxy(2, 1)
                    ##has(target, propKey)
                    用于拦截 HasProperty 操作，即在判断 target 对象是否存在 propKey 属性时，会被这个方法拦截。此方法不判断一个属性是对象自身的属性，还是继承的属性。
                    let  handler = {
                        has: function(target, propKey){
                            console.log("handle has");
                            return propKey in target;
                        }
                    }
                    let exam = {name: "Tom"}
                    let proxy = new Proxy(exam, handler)
                    注意：此方法不拦截 for ... in 循环。

                    ##construct(target, args)
                    用于拦截 new 命令。返回值必须为对象。
                    let handler = {
                        construct: function(target, args, newTarget){
                            console.log("handle construct");
                            return Reflect.construct(target, args, newTarget);
                        } }
                    class exam = {
                        constructor(name){
                            this.name = name;
                        }
                    }
                    let proxy = new Proxy(exam,handler)
                    new proxy("Tom")


                    ##deleteProperty(target, propKey)
                    用于拦截 delete 操作，如果这个方法抛出错误或者返回 false ，propKey 属性就无法被 delete 命令删除。

                    ##defineProperty(target, propKey, propDesc)
                    用于拦截 Object.definePro若目标对象不可扩展，增加目标对象上不存在的属性会报错；若属性不可写或不可配置，则不能改变这些属性。

                    let handler = {
                        defineProperty: function(target, propKey, propDesc){
                            console.log("handle defineProperty");
                            return true;
                        }
                    }plet target = {}
                    let proxy = new Proxy(target, handler)
                    proxy.name = "Tom"
                    // handle defineProperty
                    target
                    // {name: "Tom"}

                    // defineProperty 返回值为false，添加属性操作无效
                    let handler1 = {
                        defineProperty: function(target, propKey, propDesc){
                            console.log("handle defineProperty");
                            return false;
                        }
                    }
                    let target1 = {}
                    let proxy1 = new Proxy(target1, handler1)
                    proxy1.name = "Jerry"
                    target1
                    // {}

                    #erty 操作
                    getOwnPropertyDescriptor(target, propKey)
                    用于拦截 Object.getOwnPropertyD() 返回值为属性描述对象或者 undefined 。
                    let handler = {
                        getOwnPropertyDescriptor: function(target, propKey){
                            return Object.getOwnPropertyDescriptor(target, propKey);
                        }
                    }ilet target = {name: "Tom"}
                    let proxy = new Proxy(target, handler)
                    Object.getOwnPropertyDescriptor(proxy, 'name')
                    // {value: "Tom", writable: true, enumerable: true, configurable:
                    // true}

                    #ptor 属性
                    getPrototypeOf(target)
                        - Object.prototype._proto_
                        - Object.prototype.isPrototypeOf()
                        - Object.getPrototypeOf()
                        - Reflect.getPrototypeOf()
                        - instanceof
                    let exam = {}
                    let proxy = new Proxy({},{
                        getPrototypeOf: function(target){
                            return exam;
                        }
                    })
                    Object.getPrototypeOf(proxy) // {}
                        注意，返回值必须是对象或者 null ，否则报错。另外，如果目标对象不可扩展（non-extensible），getPrototypeOf 方法必须返回目标对象的原型对象。


                     let proxy = new Proxy({},{
                         getPrototypeOf: function(target){
                             return true;
                         }
                     })
                     Object.getPrototypeOf(proxy)
                     // TypeError: 'getPrototypeOf' on proxy: trap returned neither object // nor null

                     #isExtensible(target)
                     用于拦截 Object.isExtensible 操作。
                     该方法只能返回布尔值，否则返回值会被自动转为布尔值。
                     let proxy = new Proxy({},{
                         isExtensible:function(target){
                             return true;
                         }
                     })
                     Object.isExtensible(proxy) // true

                     注意：它的返回值必须与目标对象的isExtensible属性保持一致，否则会抛出错误。
                     let proxy = new Proxy({},{
                         isExtensible:function(target){
                             return false;
                         }
                     })
                     Object.isExtensible(proxy)
                     // TypeError: 'isExtensible' on proxy: trap result does not reflect
                     // extensibility of proxy target (which is 'true')

                     ownKeys(target)
                     用于拦截对象自身属性的读取操作。主要包括以下操作：
                     - Object.getOwnPropertyNames()
                     - Object.getOwnPropertySymbols()
                     - Object.keys()
                     - or...in

                     方法返回的数组成员，只能是字符串或 Symbol 值，否则会报错。

                     若目标对象中含有不可配置的属性，则必须将这些属性在结果中返回，否则就会报错。

                     若目标对象不可扩展，则必须全部返回且只能返回目标对象包含的所有属性，不能包含不存在的属性，否则也会报错。


                     let proxy = new Proxy( {
                       name: "Tom",
                       age: 24
                     }, {
                         ownKeys(target) {
                             return ['name'];
                         }
                     });
                     Object.keys(proxy)
                     // [ 'name' ]f返回结果中，三类属性会被过滤：
                     //          - 目标对象上没有的属性
                     //          - 属性名为 Symbol 值的属性
                     //          - 不可遍历的属性

                     let target = {
                       name: "Tom",
                       [Symbol.for('age')]: 24,
                     };
                     // 添加不可遍历属性 'gender'
                     Object.defineProperty(target, 'gender', {
                       enumerable: false,
                       configurable: true,
                       writable: true,
                       value: 'male'
                     });
                     let handler = {
                         ownKeys(target) {
                             return ['name', 'parent', Symbol.for('age'), 'gender'];
                         }
                     };
                     let proxy = new Proxy(target, handler);
                     Object.keys(proxy)
                     // ['name']

                     #preventExtensions(target)
                     拦截 Object.preventExtensions 操作。
                     该方法必须返回一个布尔值，否则会自动转为布尔值。

                     // 只有目标对象不可扩展时（即 Object.isExtensible(proxy) 为 false ），
                     // proxy.preventExtensions 才能返回 true ，否则会报错
                     var proxy = new Proxy({}, {
                       preventExtensions: function(target) {
                         return true;
                       }
                     });
                     // 由于 proxy.preventExtensions 返回 true，此处也会返回 true，因此会报错
                     Object.preventExtensions(proxy) 被// TypeError: 'preventExtensions' on proxy: trap returned truish but // the proxy target is extensible

                     // 解决方案
                      var proxy = new Proxy({}, {
                       preventExtensions: function(target) {
                         // 返回前先调用 Object.preventExtensions
                         Object.preventExtensions(target);
                         return true;
                       }
                     });

                     主要用来拦截 Object.setPrototypeOf 方法。
                     返回值必须为布尔值，否则会被自动转为布尔值。
                     若目标对象不可扩展，setPrototypeOf 方法不得改变目标对象的原型。
                     let proto = {}
                     let proxy = new Proxy(function () {}, {
                         setPrototypeOf: function(target, proto) {
                             console.log("setPrototypeOf");
                             return true;
                         }
                     }
                     );
                     Object.setPrototypeOf(proxy, proto);
                     // setPrototypeOf



                     ##Proxy.revocable()
                     用于返回一个可取消的 Proxy 实例。
                     let {proxy, revoke} = Proxy.revocable({}, {});
                     proxy.name = "Tom";
                     revoke();
                     proxy.name
                     // TypeError: Cannot perform 'get' on a proxy that has been revoked



             #Reflect
                ES6 中将 Object 的一些明显属于语言内部的方法移植到了 Reflect 对象上（当前某些方法会同时存在于 Object 和 Reflect 对象上），未来的新方法会只部署在 Reflect 对象上。

                Reflect 对象对某些方法的返回结果进行了修改，使其更合理。

                Reflect 对象使用函数的方式实现了 Object 的命令式操作。

                静态方法:Reflect.get(target, name, receiver)
                let exam = {
                    name: "Tom",
                    age: 24,
                    get info(){
                        return this.name + this.age;
                    }
                }
                Reflect.get(exam, 'name'); // "Tom"

                // 当 target 对象中存在 name 属性的 getter 方法， getter 方法的 this 会绑定 // receiver
                let receiver = {
                    name: "Jerry",
                    age: 20
                }
                Reflect.get(exam, 'info', receiver); // Jerry20

                // 当 name 为不存在于 target 对象的属性时，返回 undefined
                Reflect.get(exam, 'birth'); // undefined

                // 当 target 不是对象时，会报错
                Reflect.get(1, 'name'); // TypeError

                #Reflect.set(target, name, value, receiver)
                将 target 的 name 属性设置为 value。返回值为 boolean ，true 表示修改成功，false 表示失败。当 target 为不存在的对象时，会报错。
                let exam = {
                    name: "Tom",
                    age: 24,
                    set info(value){
                        return this.age = value;
                    }
                }
                exam.age; // 24
                Reflect.set(exam, 'age', 25); // true
                exam.age; // 25

                // value 为空时会将 name 属性清除
                Reflect.set(exam, 'age', ); // true
                exam.age; // undefined

                // 当 target 对象中存在 name 属性 setter 方法时，setter 方法中的 this 会绑定 // receiver , 所以修改的实际上是 receiver 的属性,
                let receiver = {
                    age: 18
                }
                Reflect.set(exam, 'info', 1, receiver); // true
                receiver.age; // 1

                let receiver1 = {
                    name: 'oppps'
                }
                Reflect.set(exam, 'info', 1, receiver1);
                receiver1.age; // 1

                #Reflect.has(obj, name)
                是 name in obj 指令的函数化，用于查找 name 属性在 obj 对象中是否存在。返回值为 boolean。如果 obj 不是对象则会报错 TypeError。
                let exam = {
                    name: "Tom",
                    age: 24
                }
                Reflect.has(exam, 'name'); // true
                Reflect.deleteProperty(obj, property)
                是 delete obj[property] 的函数化，用于删除 obj 对象的 property 属性，返回值为 boolean。如果 obj 不是对象则会报错 TypeError。
                let exam = {
                    name: "Tom",
                    age: 24
                }
                Reflect.deleteProperty(exam , 'name'); // true
                exam // {age: 24}
                // property 不存在时，也会返回 true
                Reflect.deleteProperty(exam , 'name'); // true
                Reflect.construct(obj, args)
                等同于 new target(...args)。
                function exam(name){
                    this.name = name;
                }
                Reflect.construct(exam, ['Tom']); // exam {name: "Tom"}

               # Reflect.getPrototypeOf(obj)
                用于读取 obj 的 _proto_ 属性。在 obj 不是对象时不会像 Object 一样把 obj 转为对象，而是会报错。
                class Exam{}
                let obj = new Exam()
                Reflect.getPrototypeOf(obj) === Exam.prototype // true
                Reflect.setPrototypeOf(obj, newProto)
                用于设置目标对象的 prototype。
                let obj ={}
                Reflect.setPrototypeOf(obj, Array.prototype); // true
                Reflect.apply(func, thisArg, args)
                等同于 Function.prototype.apply.call(func, thisArg, args) 。
                func 表示目标函数；thisArg 表示目标函数绑定的 this 对象；args 表示目标函数调用时传入的参数列表，可以是数组或类似数组的对象。若目标函数无法调用，会抛出 TypeError 。
                Reflect.apply(Math.max, Math, [1, 3, 5, 3, 1]); // 5

                #Reflect.defineProperty(target, propertyKey, attributes)
                    用于为目标对象定义属性。如果 target 不是对象，会抛出错误。
                 let myDate= {}
                 Reflect.defineProperty(MyDate, 'now', {
                   value: () => Date.now()
                 }); // true

                 #Reflect.getOwnPropertyDescriptor(target, propertyKey)
                 用于得到 target 对象的 propertyKey 属性的描述对象。在 target 不是对象时，会抛出错误表示参数非法，不会将非对象转换为对象。
                 var exam = {}
                 Reflect.defineProperty(exam, 'name', {
                   value: true,
                   enumerable: false,
                 })
                 Reflect.getOwnPropertyDescriptor(exam, 'name')
                 // { configurable: false, enumerable: false, value: true, writable:
                 // false}


                 // propertyKey 属性在 target 对象中不存在时，返回 undefined
                 Reflect.getOwnPropertyDescriptor(exam, 'age') // undefined

                 #用于判断 target 对象是否可扩展。返回值为 boolean 。如果 target 参数不是对象，会抛出错误。
                 let exam = {}
                 Reflect.isExtensible(exam) // true


                 #Reflect.preventExtensions(target)
                 用于让 target 对象变为不可扩展。如果 target 参数不是对象，会抛出错误。
                 let exam = {}
                 Reflect.preventExtensions(exam) // true
                 Reflect.ownKeys(target)
                 用于返回 target 对象的所有属性，等同于 Object.getOwnPropertyNames 与Object.getOwnPropertySymbols 之和。
                 var exam = {
                   name: 1,
                   [Symbol.for('age')]: 4
                 }
                 Reflect.ownKeys(exam) // ["name", Symbol(age)]


              ##组合使用
                    Reflect 对象的方法与 Proxy 对象的方法是一一对应的。所以 Proxy 对象的方法可以通过调用 Reflect 对象的方法获取默认行为，然后进行额外操作。

                    let exam = {
                        name: "Tom",
                        age: 24
                    }
                    let handler = {
                        get: function(target, key){
                            console.log("getting "+key);
                            return Reflect.get(target,key);
                        },
                        set: function(target, key, value){
                            console.log("setting "+key+" to "+value)
                            Reflect.set(target, key, value);
                        }
                    }
                    let proxy = new Proxy(exam, handler)
                    proxy.name = "Jerry"
                    proxy.name
                    // setting name to Jerry
                    // getting name
                    // "Jerry"


                ###使用场景拓展
                    实现观察者模式
                    // 定义 Set 集合
                    const queuedObservers = new Set();
                    // 把观察者函数都放入 Set 集合中
                    const observe = fn => queuedObservers.add(fn);
                    // observable 返回原始对象的代理，拦截赋值操作
                    const observable = obj => new Proxy(obj, {set});
                    function set(target, key, value, receiver) {
                      // 获取对象的赋值操作
                      const result = Reflect.set(target, key, value, receiver);
                      // 执行所有观察者
                      queuedObservers.forEach(observer => observer());
                      // 执行赋值操作
                      return result;
                    }




