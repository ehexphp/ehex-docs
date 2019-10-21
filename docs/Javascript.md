# EASY TASK - Javascript
_Located in **includes/__Php/javascript/ehex.js and .min.js** file._
This javascript file and all it classes and functions  are Automatically included in all pages unless developer chooses to turn it off.


Can be disabled by setting **$auto_page_wrapper to False in .config.php Config1::AUTO_PAGE_WRAPPER = false** or on Single Page by settings **noPageAutoWrap = true**. 
i.e 
view($view_name, $param = [], **$noPageAutoWrap = false**)
```php
// get request only
$route->get('/dashboard', function() {  
    view('user.dashboard', null, true); 
});
```


```javascript
    // list of common class in ehex.js
    function  Class1(){}
    function  Object1(){}
    function  Array1(){}
    function  Style1(){}
    function  Page1(){}
    function  String1(){}
    function  Color1(){}
    function  Cookie1(){}
    function  Form1(){}
    function  Html1(){}
    function  Math1(){}
    function  Url1(){}
    function  Picture1(){}
    function  FileManager1(){}
    function  Ajax1(){}
    function  Popup1(){}
    function  Date1(){}


    // For Instance to Print just a container in page, You can just say
    //<div id="print_me"> ... ... ... </div>
    // <button onclick="Page1.printContainer('print_me')"> Print </div>
```


