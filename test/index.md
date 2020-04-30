# regexp-test

1.  匹配出id： \<div id="container" class="main"></div>

    * 不仅匹配了id，还匹配了class
    ```js
    var regex = /id="[^]*"/g;
    var string = '<div id="container" class="main"></div>';
    string.match(regex); // [id="container" class="main"]
    ```
     
    * 惰性匹配，会回溯，效率不是最高
    ```js
    var regex = /id="[^]*?"/g;
    var string = '<div id="container" class="main"></div>';
    string.match(regex); // ["id="container""]
    ```
    * 最后
    ```js
    var regex = /id="[^"]*"/g;
    var string = '<div id="container" class="main"></div>';
    string.match(regex); // ["id="container""]
    ```

2. 数字的千位分隔符

    ```js
    var regex = /(?!^)(?=(\d{3})+$)/g;
    var string = '123123123';
    string.replace(regex, ','); // 123,123,123
    ```
