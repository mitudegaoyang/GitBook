# Lodash

[参考文档](http://lodashjs.com/docs/)
[Lodash中文文档](http://www.css88.com/doc/lodash/#_timesn-iteratee_identity)

## 简介：

Lodash是一个著名的javascript原生库，不需要引入其他第三方依赖。是一个意在提高开发者效率,提高JS原生方法性能的JS库。简单的说就是，很多方法lodash已经帮你写好了，直接调用就行，不用自己费尽心思去写了，而且可以统一方法的一致性。Lodash使用了一个简单的 _ 符号，就像Jquery的 $ 一样，十分简洁。
类似的还有Underscore.js和Lazy.js

## 浏览器支持

chrome 43往上
Firefox 38往上
IE 6-11
MS Edge
Safari 5往上
（几乎涵盖现在市面上可以见到的大部分浏览器）

## 为什么使用lodash
   
通过使用数组，数字，对象，字符串等方法，Lodash使JavaScript变得更简单。

## 入门指南

### 安装：

#### 下载lodash：

* npm安装：
```
npm install lodash
npm i --save lodash
``` 

* HTML使用script标签引入。
```angular2html
<script src="lodash.js"></script> 
```
直接下载下来引入，或者使用cdn

* node.js使用：
```angular2html
var _ = require('lodash');
```

## 常用lodash函数
####（参考版本lodash v4.16.1）

### 1、N次循环
```angular2html
<script type="text/javascript">
console.log('------- javascript -------');
//js原生的循环方法
for(var i = 0; i < 5; i++){
    console.log(i);
}
console.log('------- lodash -------');
//ladash的times方法
_.times(5,function(a){
    console.log(a);
});
</script>
```
for语句是执行循环的不二选择，但在上面代码的使用场景下，_.times()的解决方式更加简洁和易于理解。

### 2、深层查找属性值

```angular2html
<script type="text/javascript">
    var ownerArr = [{
        "owner": "Colin",
        "pets": [{"name": "dog1"}, {"name": "dog2"}]
    }, {
        "owner": "John",
        "pets": [{"name": "dog3"}, {"name": "dog4"}]
    }];
    var jsMap = ownerArr.map(function (owner) {
        return owner.pets[0].name;
    });
    console.log('------- jsMap -------');
    console.log(jsMap);

    var lodashMap = _.map(ownerArr, 'pets[0].name');
    console.log('------- lodashMap -------');
    console.log(lodashMap);
</script>
```
Lodash中的_.map方法和JavaScript中原生的数组方法非常的像，但它还是有非常有用的升级。 你可以通过一个字符串而不是回调函数来浏览深度嵌套的对象属性。

### 3、深克隆对象
```angular2html
<script type="text/javascript">
    var objA = {
        "name": "戈德斯文"
    };
    var objB = _.cloneDeep(objA);
    console.log(objA);
    console.log(objB);
    console.log(objA === objB);
</script>
```
深度克隆JavaScript对象是困难的，并且也没有什么简单的解决方案。你可以使用原生的解决方案:JSON.parse(JSON.stringify(objectToClone)) 进行深度克隆。但是，这种方案仅在对象内部没有方法的时候才可行。

### 4、在指定范围内获取一个随机值
```angular2html
<script type="text/javascript">
    function getRandomNumber(min, max){
        return Math.floor(Math.random() * (max - min)) + min;
    }
    console.log(getRandomNumber(15, 20));

    console.log(_.random(15, 20));

</script>
```
Lodash中的 _.random 方法要比上面的原生方法更强大与灵活。你可以只传入一个参数作为最大值， 你也可以指定返回的结果为浮点数_.random(15,20,true)

### 5、扩展对象
```angular2html
<script type="text/javascript">
    Object.prototype.extend = function(obj) {
        for (var i in obj) {
            if (obj.hasOwnProperty(i)) {    //判断被扩展的对象有没有某个属性，
                this[i] = obj[i];
            }
        }
    };

    var objA = {"name": "戈德斯文", "car": "宝马"};
    var objB = {"name": "柴硕", "loveEat": true};

    objA.extend(objB);
    console.log(objA); 

    console.log(_.assign(objA, objB));
</script>
```
_.assign 方法也可以接收多个参数对象进行扩展，都是往后面的对象上合并

### 6、从列表中随机的选择列表项
```angular2html
<script type="text/javascript">
    var smartTeam = ["戈德斯文", "杨海月", "柴硕", "师贝贝"];

    function randomSmarter(smartTeam){
        var index = Math.floor(Math.random() * smartTeam.length);
        return smartTeam[index];
    }

    console.log(randomSmarter(smartTeam));

    // Lodash
    console.log(_.sample(smartTeam));
    console.log(_.sampleSize(smartTeam,2));
</script>
```
此外，你也可以指定随机返回元素的个数_.sampleSize(smartTeam,n)，n为需要返回的元素个数

### 7、判断对象中是否含有某元素
```angular2html
<script type="text/javascript">
    var smartPerson = {
            'name': '戈德斯文',
            'gender': 'male'
        },
        smartTeam = ["戈德斯文", "杨海月", "柴硕", "师贝贝"];


    console.log(_.includes(smartPerson, '戈德斯文'));
    console.log(_.includes(smartTeam, '杨海月'));
    console.log(_.includes(smartTeam, '杨海月',2));
</script>
```
_.includes()第一个参数是需要查询的对象，第二个参数是需要查询的元素，第三个参数是开始查询的下标

### 8、遍历循环
```angular2html
<script type="text/javascript">
    _([1, 2]).forEach(function(value) {
        console.log(value);
    });
    _.forEach([1, 3] , function(value, key) {
        console.log(key,value);
    });
</script>
```
这两种方法都会分别输出‘1’和‘2’，不仅是数组，对象也可以，数组的是后key是元素的下标，当传入的是对象的时候，key是属性，value是值

### 9、遍历循环执行某个方法
_.map()
```angular2html
<script type="text/javascript">
    function square(n) {
        return n * n;
    }

    console.log(_.map([4, 8], square));
    // => [16, 64]

    console.log(_.map({ 'a': 4, 'b': 8 }, square));
    // => [16, 64] (iteration order is not guaranteed)

    var users = [
        { 'user': 'barney' },
        { 'user': 'fred' }
    ];

    // The `_.property` iteratee shorthand.
    console.log(_.map(users, 'user'));
    // => ['barney', 'fred']
</script>
```

### 10、检验值是否为空
_.isEmpty()
```angular2html
<script type="text/javascript">
    _.isEmpty(null);
    // => true

    _.isEmpty(true);
    // => true

    _.isEmpty(1);
    // => true

    _.isEmpty([1, 2, 3]);
    // => false

    _.isEmpty({ 'a': 1 });
    // => false
</script>
```

### 11、查找属性
_.find()、_.filter()、_.reject()
```angular2html
<script type="text/javascript">
    var users = [
        {'user': 'barney', 'age': 36, 'active': true},
        {'user': 'fred', 'age': 40, 'active': false},
        {'user': 'pebbles', 'age': 1, 'active': true}
    ];

    console.log(_.find(users, function (o) {
        return o.age < 40;
    }));
    console.log(_.find(users, {'age': 1, 'active': true}));
    console.log(_.filter(users, {'age': 1, 'active': true}));
    console.log(_.find(users, ['active', false]));
    console.log(_.filter(users, ['active', false]));
    console.log(_.find(users, 'active'));
    console.log(_.filter(users, 'active'));

</script>
```
_.find()第一个返回真值的第一个元素。
_.filter()返回真值的所有元素的数组。
_.reject()是_.filter的反向方法，不返回真值的（集合）元素

### 12、数组去重
_.uniq(array)创建一个去重后的array数组副本。

参数
array (Array): 要检查的数组。

返回新的去重后的数组
```angular2html
<script type="text/javascript">
    var arr1 = [2, 1, 2];

    var arr2 = _.uniq(arr1);


    function unique(arr) {
        var newArr = [];
        for (var i = 0; i < arr.length; i++) {
            if(newArr.indexOf(arr[i]) == -1){
                newArr.push(arr[i]);
            }
        }
        return newArr;
    }

    console.log(arr1);
    console.log(arr2);
    console.log(unique(arr1));
</script>
```
_.uniqBy(array,[iteratee=_.identity])这个方法类似 _.uniq，除了它接受一个 iteratee（迭代函数），调用每一个数组（array）的每个元素以产生唯一性计算的标准。iteratee 调用时会传入一个参数：(value)。
```angular2html
<script type="text/javascript">
console.log(_.uniqBy([2.1, 1.2, 2.3], Math.floor));
// => [2.1, 1.2]

console.log(_.uniqBy([{ 'x': 1 }, { 'x': 2 }, { 'x': 1 }], 'x'));
// => [{ 'x': 1 }, { 'x': 2 }]
</script>
```
Math.floor只是向下取整，去重，并没有改变原有的数组，所以还是2.1和1.2，不是2和1。

### 13、模板插入
```angular2html
_.template([string=''], [options={}])
```
```angular2html
<div id="container"></div>

<script src="https://cdn.bootcss.com/lodash.js/4.17.4/lodash.min.js"></script>
<script src="https://cdn.bootcss.com/jquery/3.2.1/jquery.min.js"></script>
<script type="text/javascript">
    $(function () {
        var data = [{name: '戈德斯文'}, {name: '柴硕'}, {name: '杨海月'}];
        var t = _.template($("#tpl").html());
        $("#container").html(t(data));
    });
</script>
<script type="text/template" id="tpl">
    <% _.each(obj,function(e,i){ %>
        <ul>
            <li><%= e.name %><%= i %></li>
        </ul>
    <%})%>
</script>
```
注意，这个<script>标签的type是text/template，类似于react的JSX的写法，就是js和html可以混写，用<% %>括起来的就是js代码，可以执行，直接写的就是html的标签，并且有类似MVC框架的的数据绑定，在<%= %>中可以调用到数据呈现（纯属个人见解，不知道理解的对不对）


本文的方法实例使用的lodash.js版本为4.17.3,可以在使用CDN引入或者下载新版本
_.times(_.times(number,function))
相对于for循环，lodash提供了更为高效的循环方法：_.times(number,function); 该方法会返回一个数组；
```angular2html
 var i = 0;
 var time1 = _.times(3, function(){
     console.log(i++);
     return i;
 });
```
> 输出结果为：0, 1, 2

```angular2html
 var time2 = _.times(4, _.constant(0));
 console.log(time1, time2);

```
> 输出结果为： [1, 2, 3]    [0, 0, 0, 0]

使用_.times方法创建一个有相同前缀的值的数组；
```angular2html
 var newArr = _.times(6, _.partial(_.uniqueId, 'time_'));
 console.log(newArr);

```
> ["team_1", "team_2", "team_3", "team_4", "team_5", "team_6"];

_.filter(array,fucntion(item){return //判断条件})
筛选符合条件的数组子项；返回新数组，原数组不变
```angular2html
var obj = {'data': [
         {
             'category': {
                 'uri': '/categories/0b092e7c-4d2c-4eba-8c4e-80937c9e483d',
                 'parent': 'Food',
                 'name': 'Costco'
             },
             'amount': '15.0',
             'debit': true
         },
         {
             'category': {
                 'uri': '/categories/d6c10cd2-e285-4829-ad8d-c1dc1fdeea2e',
                 'parent': 'Food',
                 'name': 'India Bazaar'
             },
             'amount': '10.0',
             'debit': true
         },
         {
             'category': {
                 'uri': '/categories/d6c10cd2-e285-4829-ad8d-c1dc1fdeea2e',
                 'parent': 'Food',
                 'name': 'Sprouts'
             },
             'amount': '11.1',
             'debit': false
         }
         ]};
         console.log(_.filter(obj.data,'debit'));
```

>
```angular2html
[
 {
     'category': {
         'uri': '/categories/0b092e7c-4d2c-4eba-8c4e-80937c9e483d',
         'parent': 'Food',
         'name': 'Costco'
     },
     'amount': '15.0',
     'debit': true
 },
 {
     'category': {
         'uri': '/categories/d6c10cd2-e285-4829-ad8d-c1dc1fdeea2e',
         'parent': 'Food',
         'name': 'India Bazaar'
     },
     'amount': '10.0',
     'debit': true
 }]

console.log(_.filter(_.filter(obj.data), function(item){
 return item.amount == 11.1;
}));
{
 'category': {
 'uri': '/categories/d6c10cd2-e285-4829-ad8d-c1dc1fdeea2e',
         'parent': 'Food',
        'name': 'Sprouts'
},
 'amount': '11.1',
     'debit': false
}

```
             
_.reject(array,function(item){return //判断条件})
取反，返回不符合判断条件的新数组，原数组不变
_.remove(array,function(item){return //判断条件})
去除符合条件的数组子项，改变原数组，返回被移除的子项组成的新数组
```angular2html
var array = [1, 2, 3, 4];
var evens = _.remove(array, function(n) {
 return n % 2 == 0;
});

console.log(array);
```

>[1, 3] console.log(evens);


> [2, 4]

_.omit(obj,arr)；
一个新的对象，移除了与数组项相同的属性；
```angular2html
 var omit = {
     'name': 'lijinglin',
     'height': 175,
     'weight': '95kg'
 }
 var newOmit = _.omit(omit, ['name','height']);
 console.log(omit, newOmit);
```
> {'name': 'lijinglin', 'height': 175, 'weight': '95kg'} 

原数组不变

> {'height':175}

清除了数组项；

_.pick(obg,function(item){});
一个新的对象，包含与数组项相同的属性；
```angular2html
 var objA = {"name": "colin", "car": "suzuki", "age": 17};
 console.log(_.pick(objA, ['car', 'age']));
```
> {"car": "suzuki", "age": 17}

_.random(min,max,floating);
返回min-max之间的随机数，如果只有一个参数，那么返回0-该值之间的随机数，最后的参数为boolean值，表示返回的数是否是浮点数；
```angular2html
 var random = _.random(10);
 var random1 = _.random(10,20);
 var random2 = _.random(10,20,true);
 var random3 = _.random(10,20,2);
 console.log(random, random1, random2, random3);
```
> 输出：
```angular2html
 10
 16
 13.626099468086885
 15.984136145057482
```

```angular2html
 var random4 = _.round(random2, 2);
 console.log(random4);
```
> 输出：
```angular2html
13.63
```
 
_.map(Collection,functoion||string);
返回一个数组。处理数组对象，当第二个参数为function改变每一项，该方法可以用于深层查找属性值；_.map方法是对原生 map 方法的改进，其中使用pets[0].name字符串对嵌套数据取值的方式简化了很多冗余的代码，


_.each(Collection,function(value,(key)){})
该方法用于数组或者对象的遍历，可以完美的代替for循环；第一个参数为需要遍历的数组或者对象，function里面的参数第一个value值必传，代表数组或对象中的每一项的值，key参数可以根据自己需求传递，表示数组或者对象中的每一项的键（key）；
_.each()方法中的continue和break：
在该方法中实现for循环break功能；只需要在方法结束时判断是否需要返回：eg：
```
 var arr = [1,2,3,4,5,6,7,8];
  _.each(arr, (item, key) ={
      console.log(item);
      if(key == 3) return false;
  });
```
> 输出1，2，3，4

效果和for循环break一致

```angular2html
  var obj = [
      {'name': 'lion', 'age': '17'},
      {'name': 'lina', 'age': '18'},
      {'name': 'luna', 'age': '23'},
      {'name': 'lich', 'age': '14'}
  ];
  _.each(obj, (item) =
 {
      if(item.age === 18) return true;
      console.log(item.name + '未成年');
  });
```

> 输出：lion未成年；lich未成年；

 效果和for循环的continue一致；
lodash更多方法请参考lodash文档：
lodashAPI
实用的lodash中文网站：http://www.itdadao.com/tags/lodash-0.html
前端常用工具：

is.js为检查数据类型等提供的小型的js库，大大减少了我们对REGEXP的依赖；
underscore.js和lodash.js提供了一整套工具函数，无需经验不足的程序员再去给内置的 JavaScript 对象打补丁；
D3.js数据可视化和图表是web应用程序的一种常规需求。当涉及到任何数据操作和可视化时，D3.js 就是事实上的标准。它是 GitHub 上最受欢迎的项目之一，并被数百个组织机构所采用。大量的图形、图标和可视化库都是构件于 D3 之上的；D3.js网站
three.jsThree.js 提供了一个轻量级的 3D 库，让你可以将 3D 效果渲染成一个 HTML5 的 canvas, SVG, 和 WebGL。在
three网站上展示了数百个漂亮的实例；
lazy.jsLazy.js是类似Underscore或Lo-Dash的JavaScript工具库,但是它有一个非常独特的特性:惰性求值，具有优良的计算性能；但是惰性计算会带来其他方便的性能问题，详见注释；故而使用惰性计算需要慎重考虑。
注释

惰性计算(Lazy Evaluation)
惰性计算是什么

函数的参数只有在需要计算是才会被计算；
一个函数不需要完全被计算只有需要的部分会被计算；
一个参数只会被计算一次；
惰性计算可以通过避免不必要的计算从而增加程序运行速率；
惰性计算的不足

由于必要的参数或则计算才会被使用，所以惰性计算在运行时，会首先判断是否需要计算，或者是否存在，从而造成另外不必要的开销； 这会导致在运行一些小的计算时，使得惰性计算并没有必要。
惰性计算在一定程度上可能造成内存泄露； 解释：惰性计算会将所有的参数计算一次，那么惰性计算会将所有计算的参数保存在内存中，当该参数不再使用后仍然可能会占用内存，这在一定程度堵上造成内存泄露的风险。
前端中的钩子使用：

百度中钩子函数的定义：
钩子函数：钩子函数是Windows消息处理机制的一部分，通过设置“钩子”，应用程序可以在系统级对所有消息、事件进行过滤，访问在正常情况下无法访问的消息。钩子的本质是一段用以处理系统消息的程序，通过系统调用，把它挂入系统。
我们要讨论的是钩子在lodash中的应用：
我理解的前端中的钩子：

前端中的钩子就是在函数或者页面调用之前对部分函数或页面内的方法进行了统一的处理，从而达到低耦合高内聚的目的；
耦合：一个软件结构内不同模块之间互连程度的度量。
内聚：一个模块内部各个元素彼此结合的紧密程度的度量。
lodash中hook的使用实例：

惰性计算的判断：

在lodash中，处理复杂的对象或者较大的数组时会使用lazy计算从而提高程序的运行性能；在调用lodash的运行方法之前例如
数组、对象、collection方法中的
at , compact , drop , dropRight , dropWhile , filter , find , findLast , head , initial , last , map , reject , reverse , slice , tail , take , takeRight , takeRightWhile , takeWhile , and toArray ；
以及_.chain()方法中的
after , ary , assign , assignIn , assignInWith , assignWith , at , before , bind , bindAll , bindKey , castArray , chain , chunk , commit , compact , concat , conforms , constant , countBy , create , curry , debounce , defaults , defaultsDeep , defer , delay ， difference , differenceBy , differenceWith , drop , dropRight , dropRightWhile , dropWhile , extend 等方法；
在调用这些方法之前会通过islaziable(func){}(版本号4.17.2第6338行)来对这些方法进行处理，从而计算出是否需要惰性计算，这些是在内部实现的，可以预处理lodash的绝大多数方法；从而增强了lodash的内聚度，而在外部函数中可以随意调用，而在外部随意调用，不要再处理。从而实现外部模块对随意调用。降低了耦合度。
lodash collection方法的使用：

collection方法中可以传入数组或者对象，lodash会通过getFuncName（）从而计算出你传入的对象，实现对目标对象的解析。大大增强了lodash中collection的方法的灵活性。