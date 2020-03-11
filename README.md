# RegExp
正则表达式方法运用

## 1、创建正则表达式方式：
* 字面量：/ pattern / flags
* 构造函数：new RegExp("pattern", "flags")，参数一个是要匹配的**字符串**模式，另一个是可选的标志**字符串**。

## 2、正则表达式字面量和RegExp构造函数的区别：
1. 对于元字符，RegExp(..)要双重转义 
2. 以前所有的正则表达式字面量都共享一个RegExp实例，现在使用正则表达式字面量和直接调用RegExp构造函数一样，每次都创建新的RegExp实例。如下代码：
3. 动态定义正则表达式时，RegExp(..)更有用。eg：`new RegExp( "\\b(?:" + name + ")+\\b", "ig" )`

| 字面量模式 | 等价的字符串(双重转义) |
| --- | --- |
| /\[bc\]at/ | "\\[bc\\]at" |
| /\d.\d{1,2}/ | "\\d.\\d{1,2}" |
| /\w\\hello\\123 | "\\w\\\\hello\\\\123 |
```
//使用正则表达式字面量必须像直接调用RegExp构造函数一样，每次都创建新的RegExp实例。IE9+、Firefox 4+和Chrome都据此做出了修改 所以返回都为true
var re = null, i;  
for (i=0; i < 10; i++){     
  re = /cat/g;     
  re.test("catastrophe"); 
} 
for (i=0; i < 10; i++){     
  re = new RegExp("cat", "g");     
  re.test("catastrophe"); 
}
```
## 3、RegExp实例属性：
* global：布尔值，表示是否设置了g标志。g：表示全局模式，即模式将被应用于所有字符串，而非在发现第一个匹配项时立即停止
* ignoreCase：布尔值，表示是否设置了i标志。i：表示不区分大小写模式，即在确定匹配项时忽略模式与字符串的大小写
* multiline：布尔值，表示是否设置了m标志。m：表示多行模式，即在到达一行文本末尾时还会继续查找下一行中是否存在与模式匹配的项
* source：字符串，按正则表达式字面量格式的字符串返回，也就是/ /内部的值
* lastIndex：number，表示下一次开始搜索的起始位置。
```
//匹配所有以"at"结尾的3个字符的组合，不区分大小写
var pattern1 = /.at/gi; 
//匹配所有".at"，不区分大小写 
var pattern2 = /\.at/gi;
```
## 4、RegExp实例方法：
* exec()
* test()
## 5、RegExp实例继承的方法：
* toString()：返回正则表达式字面量的字符串形式。与创建正则表达式的方式无关。
* toLocaleString()：返回正则表达式字面量的字符串形式。
* valueOf()：返回正则表达式本身
## 6、RegExp构造函数属性：
这些属性适用于作用域中的所有正则表达式，并且基于所执行的最近一次正则表达式操作而变化。关于这些属性的另一个独特之处，就是可以通过两种方式访问它们。换句话说，这些属性分别有一个长属性名和一个短属性名（Opera是例外，它不支持短属性名），由于这些短属性名大都不是有效的ECMAScript标识符，因此必须通过方括号语法来访问它们。下面列出了RegExp构造函数的属性：
* RegExp.input：对应短属性名$_，返回要匹配的字符串（原始字符串）
* RegExp.lastMatch：$&，最近一次匹配项
* RegExp.lastParen：$+ ， 最近一次匹配的捕获组
* RegExp.leftContext：$` ，input字符串中lastMatch之前的文本
* RegExp.rightContext：$' ，input字符串中lastMatch之后的文本
* RegExp.multiline：$* ，布尔值，表示是否所有的表达式都匹配多行模式
* RegExp.$1...RegExp.$9：分别用于存储第一、第二......第九个匹配的捕获组。
```
var text = "this has been a short summer"; 
var pattern = /(.)hort/g; 

/*  * 注意：Opera不支持input、lastMatch、lastParen和multiline属性 * Internet Explorer不支持multiline属性 */         
if (pattern.test(text)){     
  alert(RegExp.input);            // this has been a short summer     
  alert(RegExp.leftContext);      // this has been a                 
  alert(RegExp.rightContext);     // summer     
  alert(RegExp.lastMatch);        // short     
  alert(RegExp.lastParen);        // s     
  alert(RegExp.multiline);        // false 
} 
//短属性符来访问
if (pattern.test(text)){  
  alert(RegExp.$_);              // this has been a short summer     
  alert(RegExp["$`"]);           // this has been a                 
  alert(RegExp["$'"]);           // summer     
  alert(RegExp["$&"]);           // short     
  alert(RegExp["$+"]);           // s     
  alert(RegExp["$*"]);           // false 
}
```
## 7、常见正则表达式
* 元字符：模式中使用的所有元字符都必须转义，元字符包括`( ? $ * . ) \ { } [ ]`
* 零宽断言：正如它的名字一样，是一种零宽度的匹配，它匹配到的内容不会保存到匹配结果中去，最终匹配结果只是一个位置而已。
* (?:pattern)：不记录子正则表达式的匹配结果。要想捕获组不在显示就这样写，同理，反向引用（\1）也不好使了。
* (?=pattern)：零宽断言，匹配的是一个位置，该位置右侧后面紧跟着pattern字符的。
* (?<=pattern)：零宽断言，匹配的是一个位置，该位置左侧后面紧跟着pattern字符的。
* (?!pattern)：零宽断言，和?=恰好相反，(就是一个等于一个不等于)，匹配的是一个位置，该位置右侧不能跟着pattern字符。
* (?<!pattern)：零宽度负回顾后发断言来断言此位置的前面不能匹配表达式exp
* {n,}?, *?, +?, ??, {m,n}?：非贪婪者模式（尽可能少的匹配），默认贪婪者模式。
* .：会匹配字符串中除了换行符\n之外的所有字符。  
* [abc]：表示的是字符集，a或者b或者c中的任意一个字符。
* [^abc]：表示不能是a，b或者c中的任何一个  
* \f：匹配换页符  
* \n：匹配换行符  
* \r：匹配回车  
* \t：匹配制表符  
* \v：匹配垂直制表符  
* \s：匹配单个空格等同于[\f\n\r\t\v]  
* \b：匹配边界  
* \d：匹配数字  
* \w：匹配字符集合[a-zA-Z0-9_]  
* \1：反向引用匹配分组内容  

## 8、涉及正则表达式方法详解：
### 8.1、test方法
* 用法：reg.test(str)
* 返回值：返回Boolen值
* 是否忽略lastIndex：不忽略 
&nbsp;&nbsp;&nbsp;&nbsp;注意：在匹配后，reg 的 lastIndex 属性被设置为匹配文本的最后一个字符的下一个位置，同时下次，发现匹配不到任何元素，并将 lastIndex 重置为默认值 0。
```
    //校验手机号格式
    function checkPhone(str){
      return /^1\d{10}$/.test(str);
    }
```
### 8.2、exec方法：
* 用法：reg.exec(str)
* 返回值：返回数组[match1,分组match2,...,index:'匹配到的索引位置',input:'原始字符串内容']
* 是否忽略lastIndex：不忽略   
    * 如果为正则表达式设置了全局标记g，exec从上次匹配结束的位置开始查找。
    * 如果没有设置全局标志g，exec依然从字符串的起始位置开始搜索。利用这个特点可以反复调用exec遍历所有匹配，等价于match具有g标志。当然，如果正则表达式忘记用g，而又用循环（比如：while、for等)，exec将每次都循环第一个，造成死循环。
```
      var r, re; // 声明变量。 
      var s = "The rain in Spain falls mainly in the plain"; 
      re = /[\w]*(ai)n/ig; 
      while((r = re.exec(s))!=null){
          document.write(r + "<br/>"); 
          for(key in r){ 
              document.write(key + "-" + r[key] + "<br/>"); 
          } 
      }
```

### 8.3、match方法：  
* 用法：str.match(reg)
* 返回值：返回类似数组的对象，说明与exec一样，不同的是如果match的表达式匹配了全局标记g将出现所有匹配项，而不用循环，但所有匹配中不会包含子匹配项。
    * 有g：则返回 [match1,match2,match...]展示所有匹配到的项的数组。
    * 无g：则返回 [match,index:'匹配到的索引位置',input:'原始字符串内容']。
    * 无g有分组：则返回 [match1,分组match2,...,index:'匹配到的索引位置',input:'原始字符串内容']。
 ```    
var str = "cat,bat,sat,fat";
str.match(/.at/g);    //["cat", "bat", "sat", "fat"]
str.match(/.(at)/g);  //["cat", "bat", "sat", "fat"]
str.match(/.at/);     //["cat", index: 0, input: "cat,bat,sat,fat", groups: undefined]
str.match(/.(at)/)    //["cat", "at", index: 0, input: "cat,bat,sat,fat", groups: undefined]
```
### 8.4、matchAll方法：
* 返回值：返回是一个遍历器（Iterator），而不是数组。需要用for...of来遍历。与exec类似，只不过不用while循环了
```
var regex = /t(e)(st(\d?))/g;
var string = 'test1test2test3';
var matches = [];
var match;
while (match = regex.exec(string)) {
  matches.push(match);
}
matches
// [
//   ["test1", "e", "st1", "1", index: 0, input: "test1test2test3"],
//   ["test2", "e", "st2", "2", index: 5, input: "test1test2test3"],
//   ["test3", "e", "st3", "3", index: 10, input: "test1test2test3"]
// ]

const string = 'test1test2test3';
const regex = /t(e)(st(\d?))/g;// g 修饰符加不加都可以
for (const match of string.matchAll(regex)) {
  console.log(match);
}
// ["test1", "e", "st1", "1", index: 0, input: "test1test2test3"]
// ["test2", "e", "st2", "2", index: 5, input: "test1test2test3"]
// ["test3", "e", "st3", "3", index: 10, input: "test1test2test3"]
```
### 8.5、replace方法：。
* 用法：str.replace(oldstr/reg, newstr/反引用符号$1/$$/$&/$`/$'/$nn/function(match1,match2...,pos,originalText){})
* 返回值：返回新字符串
* 参数：
    * 该方法接受两个参数，第一个参数可以是RegExp对象或者是一个字符串（字符串不会被转成正则表达式），第二个参数可以是一个字符串或者是一个函数。如果第一个参数是字符串，那么它只会替换匹配到的第一项。要想替换所有匹配到的字符串，就需要提供一个正则表达式，而且要指定全局匹配(g)。
    * 第二个参数function(match1,match2...,pos,originalText)这个函数有三个参数，分别是：模式匹配的项，模式匹配项在字符串中的位置，原始字符串。在正则表达式中定义了多个捕获组的情况下，传递给函数的参数会发生变化，依次是：模式匹配的项，第一个捕获组的匹配项，第二个捕获组匹配的项......，但最后两个参数仍然是模式匹配项在字符串中的位置和原始字符串。
```     
  注意：在替换文本里$有了特殊的含义，所以我们如果想要是用$这个字符的话，需要写成$$。
  $$:代表$
  $&:代表RegExp.lastMatch
  $`:代表RegExp.rightContext
  $':代表RegExp.leftContext 
  $n:代表匹配第n个捕获组的子字符串
  $nn:匹配第nn捕获组的子字符串
```      
### 8.6、search方法：
* 用法：str.search(reg)
* 返回值：返回正则表达式第一次匹配的位置
* 是否忽略lastIndex：忽略标志 g和lastIndex属性，不执行全局匹配
### 8.7、split方法：
* 用法：str.split(str/reg,返回数组的最大长度)
* 返回值：返回一个字符串分割成的新数组。

参考文章：https://www.jianshu.com/p/81fdd0a1e7d4  
参考文章：https://www.jianshu.com/p/f09508c14e65  
参考文章：https://www.jianshu.com/p/fcb648efc3de  

