

# Form Process
Ehex provides interesting form functions

## Common Functions Are

1) The ```php Form1:: open(), addInput(), addSelect(), addTextArea() e.t.c```  and ```phpWidget:: fileDeleteBox(...), imageUploadBox(), imagesUploadBox() e.t.c .``` : Lot of function to generate form controller so you won't have to write much code an lot will be taking care automatically, @see Html Generator for more
1) ```Form1::callController( $classFunctionName = 'User::register()' )``` : This is for Form action attribute, to call a particular function on a controller that handles the form request
1) ```request()``` : this is implemented on basic $_REQUEST, buh merge $_FILES and has the ability to convert checkbox on/off to true/false and rearrange uploaded file as a normal array. e.t.c
1) ```old()``` : this has function close to laravel 'old()' buh is not implemented in the same way. this return last used $_POST/$_GET data back to app so that user won't have to re-fill form data again.



Direct Method Call in Form Action. for Example
```php
<form method="post" action="<?php echo Form1::callController(UserController::class, 'updateProfile()') ?>">
    
    <div class="row">
        <div class="col col-md-12">
            {!! HtmlForm1::addSelect('Sex', ['name'=>'sex', 'value'=>['male'=>'Male', 'female'=>'Female']]) !!}
        </div>
    </div>
    <button class="btn btn-primary btn-lg" name="btn_generate" type="submit">Generate Now</button>
</form>
```
Take a Second Look at The Form Action Above Again ```<form action="<?php echo Form1::callController(UserController::class, 'updateProfile()') ?>" >.```  
Which will direct all $_GET, $_POST, $_FILES, $_REQUEST request data to it 'UserController::updateProfile()' method or any other given method.

***
The request can then be accessed in UserController::updateProfile() method with either of $_GET, $_POST, $_REQUEST, $_FILES, or from
Ehex Custom **request()** method.

Example
```php
class UserController{
    ...
    static function updateProfile(){
   
        echo $_REQUEST['user_name'];
        // OR
        echo $_POST['user_name'];
        // OR
        echo request()->user_name;
        
        echo request()->full_name ;
        echo request()->avatar['name'] ;
    }
    ...
}
```

The flexibility of the framework also allows you to put this in the model so that all your code can be manage in one file.
Example
```php
    <form method="post" action="<?php echo Form1::callController(User::class, 'updateProfile()') ?>">
    
    <!-- OR -->
    
    <form method="post" action="<?php echo Form1::callController('User::updateProfile()') ?>">
    
    You can even pass a parameter to the method
    Form1::callController('User::updateProfile("my_param1", "my_param2", "my_param3")')

```

Request Can be access as
```php
class User{
    ...
    static function updateProfile($param1, $param2, $param3){  // for parameters
    
    
    }
    ...
```







## A Quick Full Form Could Be Made up of
```php
    HtmlForm1::open('User::formRegister()', ['method'=>'post']);

    HtmlForm1::addInput($label_name = "Full Name", $inputAttribute = ['name'=>'full_name', 'value'=>'Samson Iyanu' ]);
    HtmlForm1::addTextArea($label_name = "Description", $inputAttribute = ['name'=>'descrirption', 'value'=>'i am a developer' ]);
    HtmlForm1::addSelect($label_name = "Sex", $inputAttribute = ['name'=>'sex', 'value'=>['male', 'female'] ]);
    
    HtmlForm1::close('Send');
```

OR An Automatic Field Generator with Model1 
```php
User::formMake()->render();
// OR
$user->form()->render();
```


* HtmlForm1::open('methodName', ['formAttribute1'=>'value1']) : this generate < form action=''> tag and accept controller name directly. it has an inbuilt Form1::callController(...). 
* HtmlForm1::close('buttonValue', ['buttonAttribute1'=>'value1']) : this generate < /form> tag, it then generate submit button automatically along if button value name is passed in. default is null


```php
<div class="col-sm-12 col-md-12"> <h2>Create an Account</h2> </div>
<form method="post" action="<?php echo Form1::callController(User::class, 'formRegister()') ?>">
    <?php
        echo 
            User::formMake(null, [], ['status','role', 'avatar', 'hostel_id', 'hostel_room_number', 'last_login_at', 'created_at', 'updated_at', 'id', 'as_checked_in'])
            ->setTitle('')
            ->setSimilarFieldAttribute(['full_name', 'password', 'user_name'], ['required'])
            ->setFieldAttribute([
            'sex'=>['value'=>['male'=>'Male', 'female'=>'Female']],
            'semester'=>['type'=>'select',  'value'=>Array1::reUseValueAsKey(User::getSemester()), ],
            'level'=>['type'=>'select',  'value'=>Array1::reUseValueAsKey(User::getLevel()), ],
             ])
            ->render();
    ?>
    <input value="Signup" type="submit" />
</form>
```
And in User::formRegister()



```php
    class User extends AuthModel1 {
        public $id = -1;
        public $email = '';
        public $sex = '';
        public $avatar = '';
        public $full_name= '';
        public $user_name = '';
        public $phone_number = '';
        public $status = 'active';
        ...
        
        /**
        * Register New User
        */
        static function formRegister(){
             $result = User::register($_REQUEST, ['email', 'user_name']);
             // Upload Avatar and Login
             if($result) {
                 $result->uploadAvatar($_FILES['dp_avatar']['tmp_name']);
                 Url1::redirectIf(routes()->dashboard, ['Success', 'Account Created'], (boolean) User::login($_REQUEST['email'],  $_REQUEST['password']) );
             }
             // if Failed!
             Session1::setStatus('Failed', $result->getMessage(), 'error');
        }
        
        
        
```



## Javascript ehex.js Support

## for quick item delete. 
```blade
    <button onclick="Popup1.confirmForm('Delete Product', 'Are you sure?', '{{ Form1::callController("Product::deleteBy($productInfo->id)?token=".token()) }}')"  class="btn btn-danger"> <i class="fa fa-trash" aria-hidden="true"></i> Delete Product</button> 
```




## Upload Images and File locally
file can be uploaded to each instance of model using there primary id column to create an asset folder. upload could be done with ``` ```

```php

    /**
     * upload interface
     */
    HtmlWidget1::imagesUploadBox('images[]', $model->id? $model->getFilePath(): null) 
    
    /**
     * upload process in controller
     */
    $model = Portfolio::find(1);
    
    // Upload Many Images to model folder
    if(!String1::is_empty($_FILES['images']['tmp_name'])) {
        foreach (Array1::normalizeLinearRequestList($_FILES['images']['tmp_name']) as $file)  {
            $model->uploadFile($file);
        }
    }
    
    // then access the path with
    $allImages = $model->getFilePath();
    
    // or for access url list with
    $allImages = $model->getFileUrlList()
    
    // or image url list only
    $allImages = $model->getFileUrlList(true, FileManager1::getImageExtension(true))

```
Easy way is to use ```Model1FileLocator``` to upload. First, it keep url to all file in one place ( so plugin like ex_file_uploader could search file easily), secondly it's normalize file path automatically. Hence, made upload easier. 


```php

// Upload Many Images to model folder
if(!String1::is_empty($_FILES['images']['tmp_name']))  Model1FileLocator::uploadFiles_andInsertUrl($model, $_FILES['images']);

// then access them with
$allImages = $model->getFilePath();

```


## Upload Images to Cloud (Imgur)
```php
    // interface
    <input type="file" name="image" />
    
    
    // controller
    // try save to cloud first and the local if failed
    if(!String1::is_empty($_FILES['image']['tmp_name'])){
        $link = null;
        try{
            // save to cloud
            $link = ImgurFileManager::instance()->upload($_FILES['image']['tmp_name'])->link(); 
        }catch (Exception $ex){  
            // or save to file system
            $link = $result->uploadFile($_FILES['image']['tmp_name'])    
        }
        
        // save link to model
        if($link) $result->update(['feature_image_url'=>$link]);
        else Session1::setStatus('Image Uploading Failed', 'Failed to Upload Feature Image to Server, Please try again');
    }
```



## Form Validation
Using ```Validateor1::validate($request = null, $rules = [], $renameField = [], $messages = [], $redirect = false)```. The below code will validate for
form input ```user_name, email, age, password and avatar```, also will redirect back to form with error status automatically if the validation is failed because 
redirect parameter is set to true. Otherwise, it will return error list as array.
```php

    Validateor1::validate(
        $request = $_POST + $_FILES,
        $rules = [
             'user_name'             => 'required',
             'email'                 => 'required|email',
             'age'                   => 'required|numeric|min:18'
             'password'              => 'required|min:6',
             'confirm_password'      => 'required|same:password',
             'avatar'                => 'uploaded_file|max:2M|mimes:jpeg,png',
        ], 
        $renameField = [ // optional
            'user_name'=>'User Alias',
        ], 
        $messages = [   // optional
            'required' => ':attribute is required',
            'email' => ':email must be a validate email',
        ], 
        $redirect = true);
 
```

### Or a quick validation with just a fields key. 
```php
Validation1::validate( ['user_name', 'email', 'age'],
                [ //optional [default is required for all field]
                    'user_name'=>'required|alpha_num',
                    'email'=>'required|email',
                    'age'=>'numeric',
                ]
            )
```


### Custom similar to laravel. (check [Rakit validation](https://github.com/rakit/validation) for more)
```php
    $validator = Validator1::validator();
    
    // set request and rule
    $validation = $validator->make($_POST + $_FILES, [
         'name'                  => 'required',
         'email'                 => 'required|email',
         'age'                   => 'required|numeric|min:18'
         'password'              => 'required|min:6',
         'confirm_password'      => 'required|same:password',
         'avatar'                => 'required|uploaded_file:0,500K,png,jpeg',
         'skills'                => 'array',
         'skills.*.id'           => 'required|numeric',
         'skills.*.percentage'   => 'required|numeric',
         // OR
         'photo' => [
            'required',
            $validator('uploaded_file')->fileTypes('jpeg|png')->message('Photo must be jpeg/png image')
         ]
    ]);


    // set messages
    $validation->setMessages([
        'required' => ':attribute is required',
        'email' => ':email must be a validate email',
        'age:min' => '18+ only',
    ]);

    // validate 
    $validation = $validator->validate($inputs, $rules);
    
    // get status
    $failedStatus = $validation->fails();

    // get errors
    $errors = $validation->errors(); // << ErrorBag 
        // or error as more readable with   
            $errors = $errors->all('<li>:message</li>').
        // or only unique error for field with 
            $errors = $errors->firstOfAll(':message', false); 
```
        
### Input name array Validation
e.g 
```html
    <input type="file" name="images[profile]"/>
    <input type="file" name="images[cover]"/>
```
You can validate with images.* or with .name
```php
    $validation = $validator->validate($_FILES, [
        'images.*' => 'uploaded_file|max:2M|mimes:jpeg,png',
        // OR
        'images.profile' => 'uploaded_file|max:2M|mimes:jpeg,png',
        'images.cover' => 'uploaded_file|max:5M|mimes:jpeg,png',
    ]);
```


### More rules
```php
    'user_name'     => 'required|alpha_num'
    'email'         => 'required|email'
    'age'           => 'numeric'
    'photo'         => 'required|between:1M,2M'
    'sex'           => 'required or required_if:male,female or'
    'sex'           => 'required|in:male,female   or not_in:...'
    // OR for strict datatype check
    'sex'           => [
                        'required', 
                        $validator('in', ['male', 'female'])->strict()
                    ]
    'url' => 'url or url:http or url:http,https or url:ftp or url:mailto or url:ehexcustom must start with ehexcustom://',

```


### Callback for custom validation
```php
    $validation = $validator->validate($_POST, [
        'even_number' => [
            'required',
            function ($value) {
                // false = invalid

                if (!is_numeric($value)) {
                    return ":attribute must be numeric.";
                }
                if ($value % 2 !== 0) {
                    return ":attribute is not even number.";
                }
                // you can return true or don't return anything if value is valid
            }
        ]
    ]);
```




