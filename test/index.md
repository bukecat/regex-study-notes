# regexp-test

1. <div id="container" class="main"></div> 匹配出id

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
