# regexp-study-notes

参考：[JS正则表达式完整教程](https://juejin.im/post/5965943ff265da6c30653879)
> 正则表达式是匹配模式，要么匹配字符，要么匹配位置。

[regexper](https://regexper.com/)

## 字符匹配

1. 模糊匹配：横向模糊和纵向模糊。

    1.1. 横向模糊：可以匹配的字符串长度不是固定的，可以是多种情况。
    
    1.2. 纵向模糊：具体到某一位字符时，可以不是某个确定的字符，可以有多种可能。
    
 2. 字符组[abc]、反义字符组[^abc]、量词{m, n}。
    
 3. 贪婪匹配和惰性匹配。
    
    3.1. 贪婪匹配：尽可能的多匹配
    ```js
    var regex = /\d{3,5}/g;
    var string = "123 1234 12345 123456";
    string.match(regex); // ["123", "1234", "12345", "12345"]
    ```
    
    3.2. 惰性匹配：尽可能少匹配
    ```js
    var regex = /\d{3,5}?/g;
    var string = "123 1234 12345 123456";
    string.match(regex); // ["123", "123", "123", "123", "456"]
    ```
  4. 多选分支(p1|p2|p3)：支持多个子模式任选其一，多选分支匹配是惰性的。
  
    ```js
    // 匹配了good就不会匹配goodbye
    var regex = /good|goodbye/g;
    var string = "goodbye";
    string.match(regex); // ["good"]
    ```


## 位置匹配

1. 锚字符

    ^    （脱字符）匹配开头，在多行匹配中匹配行开头。
    $    （美元符号）匹配结尾，在多行匹配中匹配行结尾。
    \b    是单词边界，具体就是\w和\W之间的位置，也包括\w和^之间的位置，也包括\w和$之间的位置。
    \B    是\b的反面的意思，非单词边界。例如在字符串中所有位置中，扣掉\b，剩下的都是\B
    (?=p) 其中p是一个子模式，即p前面的位置。
    (?!p) 是(?=p)的反面意思
    
2. 把位置理解空字符，是对位置非常有效的理解方式。


## 正则表达式括号

1. 分组和分支结构

2. 引用分组
    ```js
    var regex = /(\d{4})-(\d{2})-(\d{2})/;
    var string = "2017-06-12";
    
    regex.test(string);
    
    console.log(RegExp.$1); // "2017"
    console.log(RegExp.$2); // "06"
    console.log(RegExp.$3); // "12"
    ```
    
    2.1. 把yyyy-mm-dd格式，替换成mm/dd/yyyy
    ```js
    var regex = /(\d{4})-(\d{2})-(\d{2})/;
    var string = "2017-06-12";
    var result = string.replace(regex, function(match, year, month, day) {
    	return month + "/" + day + "/" + year;
    });
    console.log(result); // => "06/12/2017"
    ```
    
3. 反向引用
    ```js
    var regex = /\d{4}(-|\/|\.)\d{2}\1\d{2}/;
    var string1 = "2017-06-12";
    var string2 = "2017/06/12";
    var string3 = "2017.06.12";
    var string4 = "2016-06/12";
    console.log( regex.test(string1) ); // true
    console.log( regex.test(string2) ); // true
    console.log( regex.test(string3) ); // true
    console.log( regex.test(string4) ); // false
    ```
    * 注意里面的\1，表示的引用之前的那个分组(-|\/|\.)。不管它匹配到什么（比如-），\1都匹配那个同样的具体某个字符。
    * 我们知道了\1的含义后，那么\2和\3的概念也就理解了，即分别指代第二个和第三个分组。
    * \10 表示第10个分组，而不是\1和0
    
4. 非捕获分组 (?:p)
    * 只想要括号最原始的功能，但不会引用它，即，既不在API里引用，也不在正则里反向引用。
    
## 回溯

![回溯过程](https://user-gold-cdn.xitu.io/2017/7/19/dddfffaf633dd14c4eefba488f64400f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
