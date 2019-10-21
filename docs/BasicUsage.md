# Basic Usage
Ehex (ex) cheat sheet. This is a quick reference to day-to-day use of Ehex. 




<!--
    ## Manual SQL Property Set
    https://ehex.github.io/ehex-docs/#/Model%20and%20Database?id=manual-sql-property-set 
    for more information
-->

##Model Manage
Manage Method Must return object with render method. Otherwise return null.
Copy and Modify the manage method data to suite your need.
Remember to implements Model1ActionInterface if you want it to appear in the dashboard menu
```php
class Blog extends Model1 implements Model1ActionInterface {

    // this is for content un filter
    public static $COLUMN_UN_FILTER_LIST = ['body'];
    
    // model fields
    public $name = '';
    public $slug = '';
    public $body = null;
    //...
    
    /**
     * @return mixed|Xcrud
     *  Manage Blog
     */
    static function manage(){
        return self::xcrud()
            // column edit
            ->columns('name, category_list, body, is_published, created_at')
            ->column_callback('name', [self::class, 'blogTitleAndCommentTotal'])
            ->column_cut('50', 'name, body')
            ->column_name('category_list', 'Category')
            ->column_name('is_published', 'Has Published')
            ->show_primary_ai_field(true)
            ->sum('price','align-center','Total price is {value}'); // use {value} tag to get sum value in pattern
            
            // custom column
             $xcrud->subselect('Attendance', 'SELECT count(*) FROM records WHERE user_id = {id}', 'salary') // insert as last
            
            // field type
             ->change_type('role','select', 0, ['admin'=>'Admin', 'user'=>"User"]);
             ->change_type('email','multiselect','email2@ex.com','email1@ex.com,email2@ex.com,email3@ex.com,email4@ex.com,email5@ex.com');
            ->change_type('body','editor');
            ->change_type('category_id', 'select', null, BlogCategory::selectManyAsKeyValue("", 'id', 'name'))
            
            // validation
            ->validation_required('user_name')
            ->validation_pattern('user_name', 'alpha')
            ->validation_pattern('email', 'email')
            ->validation_pattern('user_name', '[a-zA-Z]{3,14}')
            
            // photo
            ->change_type('photo','image','',array('blob'=>true));
            ->change_type('photo','image','',array(
                'thumbs'=>array(
                    array('width'=> 50, 'marker'=>'_small'),
                    array('width'=> 100, 'marker'=>'_middle'),
                    array('width' => 150, 'folder' => 'thumbs' // using 'thumbs' subfolder
                )
            ));
            
            // init field
            ->pass_default( field, value )
            ->pass_default('birth_id', Math1::getUniqueId())
            ->pass_var ( field, value, mode )
            ->pass_var('id','5', 'create')
            ->set_attr('column_name', ['id'=>'name', 'class'=>'bg-dark'])
            ->disabled('user_name','edit')
            
        
            
            // query
            ->query('SELECT * FROM users WHERE age > 25') //readonly
            ->where("users.id = 5 AND users.created_at > '{$last_visit}'")
            ->where('id =', 5) //or raw
            ->where('', " `name` like 'hello' ")
            ->where('id !', array('5','7','8'))
            ->join('profiles.token_id', 'tokens', 'id') // on profile.token_id = tokens.id
            ->relation('id','categories','pid','category_name',array('published' => 1))
            
            
            // remove button
            ->unset_title()
            ->unset_add()
            ->unset_remove()
            ->unset_edit();
            
            
            
            // highlight
            ->highlight('is_active', '=', '1', '#ff845b')
            ->highlight_row('is_active', '=', '0', '#8eff60')
                        
            // action button
            ->button(url('/blog/{id}/edit'), 'Edit', 'icon-pencil')
            ->button(url('/blog/{slug}'), 'View', 'icon-eye') 
            ->duplicate_button()
            
             // creating action. Primary Id will be in the response
             // ManageApi class is defined below
             ->before_remove(Form1::callApi(ManageApi::class, "onBlogManageDelete()?api_id=123&api_key=123"));
             ->after_insert(Form1::callApi(ManageApi::class, "onBlogManageInsert()?api_id=123&api_key=123"));
            // before_insert, after_update, before_insert, before_update        
            
            // modal
            ->modal('customerName,customerDescription')
            ->modal('customerName', 'icon-user')

    }
    
    
    // Customize XCrud Manage field
    static function blogTitleAndCommentTotal($text, $fieldName, $primaryId, $instance){ 
        return '<strong>'.$text.'</strong><br>'.'<small>('.BlogComment::count("where id = '$primaryId'").') comment</small>'; 
    }
    
    
    // event manage api class
    class ManageApi extends Api1{
    
        public static $api_id = "123";
        public static $api_key = "123";
    
        function onBlogManageDelete(){
            $id = request()->id;
            return Blog::findOrInit(['id'=>$id])->deleteAssetDirectory();
        }
    
    }
    

    
       
    
```

And you can render with

```php
    // render
    ->render(); // view
    ->render('create'); // add entry screen
    ->render('edit', 12); // edit entry screen, '12' - primary key
```

Xcrud Assets would be loaded automatically but you can choose to load it manually in a certain place with  

```php
    echo Xcrud::load_css(); //load_css() - Static method. Allow you to load xcrud's css files at you custom place. By default xcrud loads styles before instance output
    
    echo Xcrud::load_js(); //load_js() - Static method. Allow you to load xcrud's javascript files and scripts at you custom place. By default xcrud loads javascript after instance output
```


If you don't want to use the Xcrud Library, You can use the ```Array1::toHtmlTable()``` instead. It's much complicated like Xcrud but it is flexible too.
A simple example in model manage could be
```php
class User extends Model1 implements Model1ActionInterface {

    // model fields
    public $user_name = '';
    public $full_name= '';
    public $address = null;
    //...
    
    /**
     * @return mixed
     *  Manage User
     */
    static function manage(){
        return new Html1(function(){
             echo Array1::toHtmlTable(User::all(), ['user_name', 'address', 'action'], [], [], function($key, $row){
                if($key == "action")
                    return "<a class='btn btn-danger' href='".url("/$row[id]/delete")."'>Delete</a> ";
                else
                    return $row[$key];
            })
        });  
```

For More information
[Read more on Dashboard Interface](https://ehex.github.io/ehex-docs/#/Dashboard)
[Read more on Xcrud](http://xcrud.com/documentation/)







## Model Dashboard
Dashboard shows Model Information Summary. E.g Total User (10,000), Total Amount($2,000).
The Below Example Shows Stats for Blog.
Remember to implements Model1ActionInterface.
```php
class Blog extends Model1 implements Model1ActionInterface{

    public $id = 0;
    //...
    
    /**
    * @return array
    * Dashboard Menu
    */
    static function getDashboard(){
        return [
            ['name'=>'All Post',            'icon'=>'fa fa-th', 'value'=>Blog::count(), 'linkName'=>'View All', 'linkUrl'=>'/blog/manage'],
            ['name'=>'Published Post',      'icon'=>'fa fa-th', 'value'=>Blog::count('where is_published = true'), 'linkName'=>'View All', 'linkUrl'=>'/blog/manage'],
            ['name'=>'Un Published Post',   'icon'=>'fa fa-th', 'value'=>Blog::count('where is_published = false'), 'linkName'=>'View All', 'linkUrl'=>'/blog/manage'],
            ['name'=>'All Post Category',   'icon'=>'fa fa-th', 'value'=>BlogCategory::count(), 'linkName'=>'View All', 'linkUrl'=>'/blog/category/manage'],
        ];
    }
```
[Read more on Dashboard Interface](https://ehex.github.io/ehex-docs/#/Dashboard) Also Checkout [Route System](Route.md)









##Model Route and Menu
The OnRoute and Menu Works Together.
You can create model individually and implement it later just by dumbing it in your project. It appears on your dashboard automatically.
Remember to implements Model1ActionInterface.
```php
class Product extends Model1 implements Model1ActionInterface{

    public $id = 0;
    //...
    
    
    /**
     * // sidebar menu
     * @return mixed|array
     */
    static function getMenuList() {
        return [
            'Product'=>[
            
                // Custom View Route
                url('/product/create')=>'<i class="fa fa-cart"></i><span> Create Product </span>',
                url('/product/manage')=>'<i class="fa fa-gear"></i><span> Manage Product </span>',
                
                // Or Default Template Manage Route
                Dashboard::getManageUrl(self::class)=>'<i class="fa fa-gear"></i><span> Manage Product </span>',
                
            ]
        ];
    }


    /**
     * Set Route
     */
    static function onRoute($route){
        // admin only
        if(Auth1::isAdmin()){
            $route->view('/product/manage', 'pages.product.admin.manage');
            $route->get('/product/{id}/edit', function($id){ 
                return view('pages.product.admin.edit', ['id'=>$id]); 
            });
        }
        
        // all user
        $route->view('/product', 'pages.product.index');
        $route->view('/product/create', 'pages.product.create');
    }


```
[Read more on Dashboard Interface](https://ehex.github.io/ehex-docs/#/Dashboard) Also Checkout [Route System](Route.md)





##Model Process Save
Though, It is not a must to use processSave($id) method to update your model,
But it's just serve as a standard for updating model to Us. For Instance, Default User Model uses more of this, e.g processLogin(), processRegister(), 
Remember to implements Model1ActionInterface.
```php
class Product extends Model1 implements Model1ActionInterface{

    public static $COLUMN_UN_FILTER_LIST = ['tag_list'];

    public $id = 0;
    public $name = "";
    public $slug = "";
    public $body = null;
    public $feature_picture_url = "";
    public $tag_list = null;
    //...

    
    /**
     * Save  Model Information
     * @param $id
     * @return null
     */
    static function processSave($id = null){
    
        // override and convert tag_list to json string
        $_REQUEST["tag_list"] = Array1::toJSON( array_values(array_filter($_REQUEST["tag_list"], 'strlen')) );

        // create new or update if id field exists
        $result = static::insertOrUpdate($_REQUEST, ['slug']);

        // upload picture
        if($status && !empty($_FILES['feature_picture_url']['name']))
            $result->update(['feature_picture_url'=> $result->uploadFile($_FILES['feature_picture_url'], 'feature_picture.jpg') ]);

        // return with status
        return Url1::redirectWithMessage($result, '/', 'Action Successful' ,  'Action Failed:' .$status->getMessage());
    }

```
[Read more on Dashboard Interface](https://ehex.github.io/ehex-docs/#/Dashboard) Also Checkout [Api and Controller Call](Api%20and%20Controller%20Call.md)


##Quick Login with Ajax
your login.blade.php
```html
     {!! HtmlForm1::open("User@processLogin()", ['class'=>'login', 'id'=>'login_form']) !!}
        {!! HtmlForm1::addInput('Email / User Name', ['name'=>"user_name", ""]) !!}
        {!! HtmlForm1::addInput('Password', ['name'=>"password", 'type'=>'password', 'toggle'=>'true', ""]) !!}
     {!! HtmlForm1::close('Login', ['class'=>'btn btn-lg btn-block btn-primary']) !!}

    <script>
        Ajax1.submitForm("login_form", "<br/>Signing in...", (result)=>{
            if(result.status) {
                Popup1.alert("Login Success", result.data, 'success');
                Url1.redirect("{{ routes()->dashboard }}");
            }
            else Popup1.alert("Login Failed", result.data, 'error');
        });
    </script>
```
and process method ```User@processLogin()``` in User model your User.php
```php
    
    class User extends Model1{

        static function processLogin(){
            // validate
            $validateError = Validation1::validate( ['user_name', 'password'], ['user_name'=>'required',  'password'=>'required|min:6'], [], []);
            if($validateError) return ResultObject1::make(false, "Validation Failed", $validateError);
    
            // login
            $result = User::login($_REQUEST['user_name'], $_REQUEST['password'], ['user_name', 'id', 'email'], ['password'], true);
            return ResultObject1::make(!!$result, "Completed");
        }
    ...

```
 
##Quick Form validation
Using ```Validateor1::validate($request = null, $rules = [], $renameField = [], $messages = [], $redirect = false)```
```php
    /**
     * @param null $request  e.g $_POST + $_FILES
     *
     * @param array $rules [
     *       'name'                  => 'required|alpha_num',
     *       'email'                 => 'required|email',
     *       'age'                   => 'required|numeric|min:18'
     *       'password'              => 'required|min:6',
     *       'confirm_password'      => 'required|same:password',
     *       'avatar'                => 'required|uploaded_file:0,500K,png,jpeg',
     *       'skills'                => 'array',
     *       'skills.*.id'           => 'required|numeric',
     *       'skills.*.percentage'   => 'required|numeric',
     *        OR
     *       'photo' => [
     *          'required',
     *          $validator('uploaded_file')->fileTypes('jpeg|png')->message('Photo must be jpeg/png image')
     *       ]
     * ]
     *
     *
     * @param array $renameField [
     *      'province_id' => 'Province',
     *      'district_id' => 'District'
     * ]
     * @param array $messages [
     *      'required' => ':attribute is required',
     *      'email' => ':email must be a validate email',
     *      'age:min' => '18+ only',
     *
     * ]
     * @param boolean $redirect
     * @return bool|null
     *
     * public static function validate($request_or_keys = null, $rules = [], $renameField = [], $messages = [], $redirect = false);
     */
    
    // Example
    Validation1::validate( ['user_name', 'email', 'age'],
        [
            'user_name'=>'required|alpha_num',
            'email'=>'required|email',
            'age'=>'numeric',
        ]
    )

```
[Read more on Form Validation](https://ehex.github.io/ehex-docs/#/Form)
[Read more on Form Validation](https://ehex.github.io/ehex-docs/#/BasicUsage#Quick%20Form%20validation)
