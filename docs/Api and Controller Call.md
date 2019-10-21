# Api and Controller


## Accessing Api and Controller Class ( the CLF pattern ).
Calling Rest Api with some line up link looks boring and not friendly. Therefore, we code out our own api link calling style called "call like a function" (CLF).  
which can access class method directly in a secure way. therefore, You can call any static method of model, controller or api class on the go, simply by adding ```?token=token()``` param or Api_key and Api_Id
i.e instead of 
writing your api link as  

Example 1

Instead of
```
    https:://site-name.com/api/calculator/add/2/5
```
You can use ehex clf pattern.

```
    https:://site-name.com/api/Calculator::add(2, 5)
```
Note: The Url is case-insensitive. and the "::" symbol can be replaced with "@" symbol
This will create a url link to *Calculator* Api1 extended class and it method *add(2, 5)*.
Since we are already used to the way of calling class methods, This Makes it more Readable and Understandable also Secured (because it requires session crsf token or api_id/api_key) to access any class method. 
***
Example 2
```
    https:://site-name.com/api/calculator/add?a=2&b=5
```
You can just use

```
    https:://site-name.com/api/Calculator::add()?a=2&b=5
```


Calling a module of code in Api1 extended class is closely the same as calling your Controller1 extended class.
i.e, 
Api can either be accessed by calling Form1::callApi('UserApi::getFollowers()?token='.token()), or manually with  url('api/UserApi::getFollowers()?token='.token())



## Api Form Field Sample
```php
    
    <div class="form-group"> 
        Select Category Or <a class="addNewCategory" href="#"> Add Category </a>
        <select class="form-control" name="category_id" id="category_id">
            <option value="1">The James</option>
            <option value="2">Past Story</option>
        </select>
    </div>

                 
                 
    <script>
        /**
        *  Add New Category
        */
        $('.addNewBlogCategory').click(function(event){
            
            Popup1.input('Input Name', 'input the category name', null, function(input){
   
                Ajax1.requestGet("{!! Form1::callApi(Blog::class, "addBlogCategory()?user_id=$userInfo->id&token=".token() ) !!}&title=" + input, null, function(data){
                    data = Object1.fromJsonString(data);;
    
                    if(data.status) { 
                        // if action is successful
                        $('#category_id').append('<option value="' + data.data.id + '">' + data.data.title + '</option>');
                        swal( 'Category Added', 'You have successfully added category "' + data.data.title + '"' );
    
                    }else{ 
                        // else, throw message
                        swal( 'Failed to Add', data.message );
    
                    }
                });
            });
        });
    </script>                           
    ...
```

This Javascript display a Popup with EX inbuilt JavaScript (Popup1), Which will request for category name to be added.
The Ex inbuilt Ajax1 function (a well define ajax function for EX) accept the Popup1 value and process the request with the api link given.
The **{!! Form1::callApi(Blog::class, "addBlogCategory()?user_id=$userInfo->id&token=".token() ) !!}** is just a direct link to api route,
i.e you will have something like *"http://site-link.com/api/Blog::addBlogCategory()?user_id=$userInfo->id&token=uh32igk2fc23u10af9893hf"* Automatically. And this will call a method 
that will add new Category to the database.



## Api Controller Sample
```php
    class BlogApi extends Api1 {
        ...
            /**
             * @return ResultObject1
             * 
             * Api Call to Add new Category for Blog on the fly
             */
            static function addBlogCategory(){
                $result = BlogCategory::insert(request(), ['title', 'user_id'], 'AND' );
                return ResultObject1::make($result, $result->getMessage(), $result? $result->toArray(): null);
            }
        ...
        
    ...
```




> Why Controller can also be called with ```Form1::callController('User::addFollowers()')```, or manually with ```url('/form/UserApi::getFollowers()')``` in your form action, always include **token=token()** field in your hidden field for "cross site request forgery (csrf attack)"


## Form Field Sample
```php
    <form action="{{ Form1::callController(Blog::class, 'processSave()') }}" method="post" enctype="multipart/form-data">

    {!! form_token() !!}

    {!! HtmlForm1::addInput('<i class="fa fa fa-bookmark"></i> Url Slug',       ['name'=>'slug', 'value'=>"$blog->slug", 'type'=>'text', 'onkeyup'=>'autoSlug = false']) !!}
    {!! HtmlForm1::addInput('<i class="fa fa-calendar"></i> Published Date',    ['name'=>'published_at', 'value'=>String1::isSetOr($blog->published_at, now()), 'type'=>'datetime']) !!}
    {!! HtmlForm1::addInput('<i class="fa fa-key"></i> Lock with Password',     ['name'=>'password', 'value'=>"$blog->password", 'type'=>'password']) !!}

```

## Form Controller Sample
```php
class BlogController extends Controller1 {
    ...
    static function processSave(){
        $result = Blog::insert(request(), ['slug']);
        if($result){
            $result->uploadFile(request()->image, $result->id);
            Session1::setStatus('Blog Saved', $result->getMessage(), 'success');
        }else{
            Session1::setStatusFrom($result);
        }
    }
...
}
```


## Restrict CLF Access 
For more Security purpose, restriction should be put on CLF access, i.e only put the method you want into ```$CLF_CALLABLE_LIST['methodName']```. default is ```public static $CLF_CALLABLE_LIST = ['*'];```, all the parent class(Model1) methods is available for your model to use out-of-box.
Assuming a Model User requires only ```processVerifyUser``` method to be called as api or controller, then you should just add only ```processVerifyUser``` method to your ```$CLF_CALLABLE_LIST = ['processVerifyUser']``` i.e just static method name.
```php
    class User extends AuthModel1 implements Model1ActionInterface {
        // clf restriction sample 
        public static $CLF_CALLABLE_LIST = ['processVerifyUser', 'processRegister', 'processLogin', 'processSave'];
        
        static function processVerifyUser(){
            // do anytin here
        }
        
        ...
```
This makes other methods inaccessible as CLF. The common method that usually required this is your ```$CLF_CALLABLE_LIST['processSave', 'processUpdatePage']```,


You can decided to make any method accessible temporarily through ```bypassToken()``` method, or with  ```Form1::callControllerAndBypassToken(token(), 'class@methodName()')```, or Form1::callApiAndBypassToken  for Api. 
this way, you are telling the system to byPass security token temporarily. bypassToken should only be used on a safe method.
Another easiest way to do this is with ```Url1::actionLink```, e.g maybe on a link like ```<a href='Url1::actionLink("ContactUs@processSave()?phone_number=+2348064816493")'>Save +2348064816493 Number</a>```,
This will not require token at all.



## CLF Bypass Token
Request token for a method can be ignored temporarily  or permanently for a class method.
> To Ignore token for a method temporarily please use ```bypassToken()```  function. i.e instead of using 
```Form1::callController( methodName )```, please use ```Form1::callControllerAndBypassToken(token(), methodName)```  to remove token from request url.
and  ```Form1::callApiAndBypassToken(token(), methodName)```  for api too. 
The bypassToken function confirm the session token and store the request method in a session in a secured way.
Also The function can be called manually with ```ServerRequest1::bypassToken(...)```  and can be verified with ```ServerRequest1::validateCLFAndBypassedToken(...)```

> To Ignore token for a class method permanent please insert a static variable in the class.  ```$CLF_BYPASS_TOKEN_LIST = []``` , This will contain an array of method to be bypassed. i.e in user, to 
verify user email address requires token but we want to ignore it. Therefore, How class will consist of  ```$CLF_BYPASS_TOKEN_LIST = ['processVerifyAccount']```
```php
    class User extends AuthModel1{
        public static $CLF_BYPASS_TOKEN_LIST = ['processVerifyAccount'];
        
        /**
        * Verify Registered User
        * @param string $email
        */
        static function processVerifyAccount($email = ''){
            $email = @decode_data($email);
            $result =  $email? User::find($email, 'email'): null;
            if($result) $result->update(['status'=>'active']);
            redirect('/dashboard', [
                $result? 'Account Verified!': 'Failed to Verify Account', 
                $result? 'Welcome back '.$result->full_name.', You are now a full member of '.Config1::APP_TITLE: 'Failed to Verify your Account', 
                $result? 'success': 'error'
            ]);
        }
        ...
        
```









## Using ex Javascript Form1.sendPost
> Quick form  request could be made with ```Form1.sendPost( actionUrl, postDataValues, method, target, traditional, redirectTop )``` example is    ```Form1.sendPost("/form/User@add()", { user: "johnDoe", password: "12345"});``` and this will create a new user johnDoe
> This will generate form element and generate input field for all parameter and submit the form automatically.

```php
    <button onclick="Form1.sendPost('{{ Form1::callController("Product@setActive(1, true)") }}')" > Make Active </button>
```
Please include token in any form related request.


## Get  Method Parameter + Request with  $model::request()['key_name']
To get available parameter from either method parameter or form request(post/get fields), Use ```self::request()['key_name']```. or simple ```self::request()->key_name```
> your model::request() contains the combination of current method parameter and $_REQUEST parameter. Please note. ```$_REQUEST``` parameter is a fixed field in  ```self::request()['key_name']```, therefore,
it will overrides method parameters.

```php
Ajax1.request("<?= url('/api') ?>/User::createData('samtax', 'samson iyanu')?token=<?=token()?>", {age:20, sex:'male'}, function(result){
    dd(result);
});
```
>  and in User Model
```php

    function createData($user_name, $full_name){

        dd( self::request() );
        
        // output will be 
        user_name = 'samtax',
        full_name = 'samson iyanu',
        age = 20,
        sex = 'male',
        token = '....',

    }

});
```
This is very good and necessary because it offers all in on place. Therefore, 
we recommend using the ```self::request()``` in situation where you can you can either get value through method parameters
or as a $_GET or $_POST request.




## Redirect location After CLF Method Execution
You will be redirected back to previous link after  CLF method (call like a function method) execution,
> Example in
```php
    <form action="{{ Form1::callController(Blog::class, 'processSave()') }}" method="post">
```
i.e. After calling  form action  ```Blog::processSave()```, By Default, app redirected back to previous page where the request is being called. to change this, it's either you call ```redirect(...path...)``` manually in your method or
pass a  ```redirect_to``` field to your request.

> Sample with redirect_to field.
```php
    <form action="{{ Form1::callController(Blog::class, 'processSave()').'?redirect_to='.url('/dashboard') }}" method="post">

    // OR

    <form action="{{ Form1::callController(Blog::class, 'processSave()') }}" method="post">
    <input type='hidden' name='redirect_to' value='{{ url('/dashboard') }}'>
```







## Load Content Dynamically
To Speed up web page loaded time, document could be fetched dynamically using  ajax implement features of  ```Url1```  i.e ```Url1::loadContentByAjax($url, $optionalFieldName = null)```   or Using ```exUrl1::loadContentByAjax($url, $optionalFieldName = null)``` 
```php
    Url1::loadContentByAjax( Form1::callApi("User::getField(1, 'full_name')?token=".token()) );
    
    //or
    
    Url1::loadContentByAjax( Form1::callApi("User::find(1)?token=".token()), 'full_name' );
```

> Example

```php    
    
    // auto fetch and paste plain content
    Url1::loadContentByAjax( Form1::callApi("User::getField(1, 'full_name')?token=".token()) );
    
    
    // fetch input field value content
    <input id='textBox' style="width:500px" />
    Url1::loadContentByAjax( Form1::callApi("User::getField(1, 'avatar')?token=".token()), null, 'textBox', 'val' );
    
    
    // fetch and render content with pre text value 
    Url1::loadContentByAjax( Form1::callApi("User::getField(1, 'full_name')?token=".token()), null, null, "My Fullname is %s" );
    
    
    // fetch content with full tag, e.g image content. Just put $s in place of value
    Url1::loadContentByAjax( Form1::callApi("User::getField(1, 'avatar')?token=".token()), null, null,  '<img style="width:500px;height:500px;"  src="%s"/>');
    
    <h3>Full Name</h3>
    <p><?php  Url1::loadContentByAjax( Form1::callApi("User::getField(1, 'full_name')?token=".token()) ); ?></p>
    
    <h3>About Author</h3>
    Url1::loadContentByAjax( Form1::callApi("User::find(1)?token=".token()), 'about' );
```




## Api Security
To Access Api class or Api extended class (like Api1,  Model1 or Controller1 ) with CLF request, a token field or api_key and api_id is required. 
> ```token``` usually for internal use. i.e use in the same app.
token field must be present in the request field. get token value from ```token()``` method or ```form_token()``` widget, which will consist of hidden token field.
```php
    <form action="{{ Form1::callController('User::processSave()') }}" >
        ...
        {!! form_token() !!}
        
        or 
        
        <input name='token' value='{{ token() }}' />

        ...
    </form>

```


> ```$api_key``` and ```$api_id``` this can also be declared in Api Class as Security Key for accessing class method.
```php
    class MyApi extends Api1 /* or Model1 or Controller1 */{
        ... 
        public static $api_id = 'my_id';
        public static $api_key = '12345';
        ...
    }
```

> Additional Security are ```isApiAuthValid()``` which tells if token is valid, and ```isUserAllowed()``` which tells the type of user to allow, This can
also tells if the user has additional requirement like it's own personal api_key or other requirement.
```php
    class MyApi extends Api1 /* or Model1 or Controller1 */{
        ...
        // Main and Default Validation
        public static function isApiAuthValid(){
            return isset($_REQUEST['token'])? is_token_valid($_REQUEST['token']): false;
        }
        
        // Allow specific user  either by role colomn or other specification
        public static function isUserAllowed(){
            return !!User::getAllowedRoleLogin(['admin', 'staff']);
            
            // or default
            // return true;
        }
        ...
    }
```

## Api lifeCycle
Before calling the actual api method through CLF, you might want to validate some data, or token() with JWT or set some initial variable.
Currently, we have ```onApiStart($request)``` that is first called before the actual method and which must return true. 
example could be : 
> let say i am calling  url("/api/MyApi@updateUser(1, full_name, 'Mr Lawrence')?token=".token()) through ajax request. It means
>the method will first reach onApiStart() method and if onApiStart returns true, it the proceed to updateUser() method other throw permission denied error
```php
    class MyApi extends Api1 {

        // on api request start
        public static function onApiStart($request){ 
            return true;
        }

        // the actual method
        public static function updateUser($user_id, $field, $value){ 
            return true;
        }
        
```


## Action Link
You can access a function quickly with ```Url1::actionLink()```, it's a quick alternative to ```Form1::callControllerAndBypassToken()```. This will give you a direct link to any method in your Model, Api or Controller extended class.
Example : To get a direct link access to a class function 
e.g
```php
    class Calculator{
        static function add($a, $b){
          dd($a + $b)
        }
        ...
    }
```

A direct link to function add() of above Calculator class will be 
```php
    echo Url1::actionLink(Calculator::class, "add(2, 5)")
```

will output 
```php
 https:://site-name.com/api/Calculator::add(2, 5)
```

An E-commerce  Add to Cart could be 
```html
    <a href="{{ Url1::actionLink(Cart::class, "add($product->id)") }}">Add to cart</a>
```



## Please, Note.
All CLF request parameter are treated as either number/string/bool/null value, 
Therefore : 
 * You cannot use Object or an Array as request Args. Convert it to JSON String instead.
 

























