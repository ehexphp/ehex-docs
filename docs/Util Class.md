
# Summary Util Class
We Suffixed All Classed with "1" to avoid class conflict and ensure consistency since we are not using any __NameSpace__.
Any Easy Task **Class Consist of "1" suffix** including javascript library, 
Others Helpers fom Ehex includes **suffix "2" for Laravel** e.t.c


> See [Check full api](https://xamtax.com/easytask-api)
***
Util Class Example Includes

1. ResultStatus1
1. ResultObject1
1. Page1
1. Value1
1. Chat1
1. Html1
1. ServerRequest1
1. RegEx1
1. Validation1
1. String1
1. Converter1
1. Array1
1. RecursiveArrayObject1
1. ArrayObject1
1. Class1
1. Object1
1. Console1
1. Log1
1. Color1
1. FileManager1
1. Form1
1. DateManager1
1. Date1
1. SessionPreferenceSave1
1. Header1
1. Url1
1. Number1
1. Math1
1. Cookie1
1. Session1
1. Popup1
1. Picture1
e.t.c


***
Example :

```php
    // String1 get Demo Text
    echo  String1::getDemoText(20)
    output: eaque ab et Suscipit
    
    
    
    // Math1 get Unique Id
    echo  Math1::getUniqueId()     
    output: 1536494216938
 
 ```   


 ***
Check Util Documentation for more... 
***[Util Class](http://ehex-api.xamtax.com)***
 ***



 ```php
     //Under String1
    
        static function decodeUnicode($text)
    
        static function encodeStringToNumber($string)
        
        static function decodeStringBackFromNumber($stringNumber)
    
        static function encodeToShortAlphaNum($string)
        
        static function decodeFromShortAlphaNum($string)
    
        static function getSubString($text, $length, $start = 0)
    
        static function getSomeText($text, $length='20', $ellipsis = '...')
    
        static function contains($needle, $haystack) 
    
        static function containsMany($needles = [], $haystack, $operator = 'or', $asWord = false)
    
        static function toBoolean($value, $trueValue = true, $falseValue = false) 
        
        //E.T.C
 
 ```   
