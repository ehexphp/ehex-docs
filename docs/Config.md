# EASY TASK - Config1 class
_Located in **.config.php** file._ first file in the root page

This consist of the entire settings of Ehex Framework, Including Database settings, Mail, Language Translator, App Meta data like title and description, life-cycle methods like onPageStart(), onPageEnd(), onRoute() e.t.c.



## Config1::onRoute() 
Config method name is Config1::onRoute($route)
All "View Blade" (Html Page) are given a unique name use to access them which can be change anytime without changing the view name itself. This provides lot of flexibility to the view and helps others to understand/remember each website page link.
_Located in **.config.php >> config1 onRoute($route) method** 



    

## Config1::onPageStart()
Run on every Page before anything on the page. Also return Global Shared Variable by developer to any page. For Example
```php
    // declare shared variable on config1 onPageStart
    static function onPageStart() { 
        return  [
            'full_name' => 'Samson Iyanu',
            'user_name' => 'Samtax',
            'phone_number' => '08064816493',
        ]; 
    }
    
    
    // use in any page as
    echo $full_name;
    echo $user_name;
    ...
```


## Config1::onPageEnd()
Run after Page has executed completely. Maybe you would like to update datebase or something



## Dynamic Config Variable
To access variable dynamically. Put this ```config_init($keyValueArray = [])``` in your config onPageStart() or at the template top to override the default config const variables.
```php
    // declare shared variable on config1 onPageStart
    static function onPageStart() { 
        config_init([
            'MAIL_HOST'=>'mail.ehex.com'
            'MAIL_EMAIL'=>'hello@ehex.com'
            'MAIL_PASSWORD'=>'123456'
        ]);
        ...
``` 
Then in your app, just call ``` config('MAIL_HOST')  ``` .
When trying to get config property, The config($key) first check the $__ENV global variable,
if not found, it then check the config class const variables if const is defined, otherwise, it just use the default_value.
Override the config from the app startup page with config_init($keyValueArray)
E.T.C



 ***
Check Documentation for more... 
***[Util Class](https://ehex.xamtax.com/config)***
 ***