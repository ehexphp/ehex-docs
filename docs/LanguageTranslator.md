# Language Translator Class.

## Introduction
Easy and advance language switch has been implemented into ehex. the function is located inside the config file (```Config1::language()```)
It only requires you to init ```set_language(default, new)``` at the top of the page, where "default" is the active language the app is created with, and  "new" is 
the new language the user selected. For instance, The user might switch language from english in language dropdown box to russia, The developer is expected to first
save it to session and then pass the selected language to ```set_language('en', 'ru')```. en = English and ru = Russia.
and Therefore all language would be pointed to russia henceforth. 


## init
> Insert all language combination into config1 file. I.e
```php
    class Config1{
        ...
        static function language(){
                return [
                    'lost_password'=>'Lost Your Password',
                    'confirm'=>'Are you sure?',
                    'signup'=>  [
                        'default'=>'Register',
                        'ru'=>'kola',
                        'yr'=>'dara pomo wa',
                    ],
                    'login'=>  [
                        'default'=>'Login',
                        'ru'=>'adLouqd',
                        'yr'=>'wole si wa',
                    ],
                ];
        }
    ...

```
Then at the beginning of your page or template, insert ```set_language('en', isset($_SESSION['language'])? $_SESSION['language']: 'en' )``` and 
when the session language is changed, the language translator pointer response to it.


## Usage
To get the current pointed language use ```get_language('signup')``` or ```__('signup')```
You can also translate a new word that does not exists yet with ```__('Hello world')``` .

## Custom Usage
You can translate any words with ```String1::translateLanguage(...)``` or array of words in key value format ```String1::translateLanguageKeyValue(...)```

