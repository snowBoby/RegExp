# RegExp
正则表达式方法运用

(?:pattern)：不记录子正则表达式的匹配结果。

(?=pattern)：零宽断言，匹配的是一个位置，该位置右侧后面紧跟着pattern字符的。

(?<=pattern)：零宽断言，匹配的是一个位置，该位置左侧后面紧跟着pattern字符的。

(?!pattern)：零宽断言，和?=恰好相反，(就是一个等于一个不等于)，匹配的是一个位置，该位置右侧不能跟着pattern字符。

(?<!pattern)：零宽度负回顾后发断言来断言此位置的前面不能匹配表达式exp

零宽断言：正如它的名字一样，是一种零宽度的匹配，它匹配到的内容不会保存到匹配结果中去，最终匹配结果只是一个位置而已。

{n,}?, *?, +?, ??, {m,n}?：非贪婪者模式（尽可能少的匹配），默认贪婪者模式。

.：会匹配字符串中除了换行符\n之外的所有字符。

\f匹配换页符，\n匹配换行符，\r匹配回车，\t匹配制表符，\v匹配垂直制表符，\s匹配单个空格，\d匹配数字，\w匹配字符集合[a-zA-Z0-9_],\1反向引用匹配分组内容。

注意：在匹配后，rgExp 的 lastIndex 属性被设置为匹配文本的最后一个字符的下一个位置。lastIndex并不在返回对象的属性中，而是正则表达式对象的属性。

1、test方法：返回Boolen值。不忽略lastIndex。  格式：reg.test(str)
  //校验手机号格式
  function checkPhone(str){
    return /^1\d{10}$/.test(str);
  }

2、exec方法：返回数组[match1,分组match2,...,index:'匹配到的索引位置',input:'原始字符串内容']。不忽略lastIndex。 格式：reg.exec(str)。
      exec默认只返回匹配结果的第一个值，无论re表达式用不用全局标记g。如果为正则表达式设置了全局标记g，exec从上次匹配结束的位置开始查找。如果没有设置全局标志，exec依然从字符串的起始位置开始搜索。利用这个特点可以反复调用exec遍历所有匹配，等价于match具有g标志。当然，如果正则表达式忘记用g，而又用循环（比如：while、for等)，exec将每次都循环第一个，造成死循环。
      var r, re; // 声明变量。 
      var s = "The rain in Spain falls mainly in the plain"; 
      re = /[\w]*(ai)n/ig; 
      while((r = re.exec(s))!=null){
          document.write(r + "<br/>"); 
          for(key in r){ 
              document.write(key + "-" + r[key] + "<br/>"); 
          } 
      }


3、match方法：返回类似数组的对象。 格式：str.match(reg)  
      说明与exec一样，不同的是如果match的表达式匹配了全局标记g将出现所有匹配项，而不用循环，但所有匹配中不会包含子匹配项。
      有g：则返回 [match1,match2,match...]展示所有匹配到的项的数组。
      无g：则返回 [match,index:'匹配到的索引位置',input:'原始字符串内容']。
      无g有分组：则返回 [match1,分组match2,...,index:'匹配到的索引位置',input:'原始字符串内容']。
     
      var str = "cat,bat,sat,fat";
      str.match(/.at/g);    //["cat", "bat", "sat", "fat"]
      str.match(/.(at)/g);  //["cat", "bat", "sat", "fat"]
      str.match(/.at/);     //["cat", index: 0, input: "cat,bat,sat,fat", groups: undefined]
      str.match(/.(at)/)    //["cat", "at", index: 0, input: "cat,bat,sat,fat", groups: undefined]

4、replace方法：返回新字符串。 格式：str.replace(oldstr/reg, newstr/反引用符号$1/function(match1,match2...,pos,originalText){})
      该方法接受两个参数，第一个参数可以是RegExp对象或者是一个字符串（字符串不会被转成正则表达式），第二个参数可以是一个字符串或者是一个函数。如果第一个参数是字符串，那么它只会替换匹配到的第一项。要想替换所有匹配到的字符串，就需要提供一个正则表达式，而且要指定全局匹配(g)。
      第二个参数function(match1,match2...,pos,originalText)这个函数有三个参数，分别是：模式匹配的项，模式匹配项在字符串中的位置，原始字符串。在正则表达式中定义了多个捕获组的情况下，传递给函数的参数会发生变化，依次是：模式匹配的项，第一个捕获组的匹配项，第二个捕获组匹配的项......，但最后两个参数仍然是模式匹配项在字符串中的位置和原始字符串。
      注意：在替换文本里$有了特殊的含义，所以我们如果想要是用$这个字符的话，需要写成$$。

5、search方法：返回正则表达式第一次匹配的位置。不执行全局匹配，它将忽略标志 g和lastIndex属性。格式：str.search(reg)。

6、split方法：用于把一个字符串分割成字符串数组。格式：str.split(str/reg,返回数组的最大长度)。

参考文章：https://www.jianshu.com/p/81fdd0a1e7d4
参考文章：https://www.jianshu.com/p/f09508c14e65
参考文章：https://www.jianshu.com/p/fcb648efc3de

