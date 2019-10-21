
# Model and Database
  You won't need to keep writing  SQL Query for everything anymore. This help beginners to stay out of Common SQL Syntax error and also helps to concentrate on other important code instead of re-writing/fixing sql codes, 
  With Ehex Model1, You can 
  1. Create or Delete Database
  1. Create Table, Delete Delete, Truncate, Insert Data to Table E.T.C Automatically
  
  
    
## Perform Sql Operation on Model
 ```*
 Both Database and table Are Setup for Automatic Creation in ```.config.php >> config1 onDebug() method, This Method get executed each time the page re-loads While $debug_mode is set to true``` Therefore, You should always remember to turn it off for Production.
 
 ```php
        /```
         * Run Model Create, Alter or Destroy Here When Debug is TRUE in ".config.php"
         */
        static function onDebug(){
            // Sample
    
            // Delete Table
            User::tableDestroy();
    
            // Create Table from Php Class (with the help of class variables. Must Extend Model1 Class)
            User::tableCreate();
    
            // Clone Table rows
            User::tableDataClone();
    
            // Clear Table data
            User::tableTruncate();
            
            
    
         
            // Rename Table Column Name
            // e.g User::tableColumnRename([ "alias_name"=>"user_name", "email_address"=>"email"])
            // or with more attribute User::tableColumnRename([ "alias_name"=>"user_name varchar(50)" ])
            User::tableColumnRename();
    
            // Re-Adjust Table Columns, Maybe fields are been Deleted/Added to Model
            // Each time you change a model property, i.e, a model class variable, run ```tableReset()``` on model to add/delete the new variable to table column
            // Synchronize table with it's model.
            //      Soft-reset will Re-Adjust model fields to match with db table,
            //      While forceReset/Hard-Reset will delete and re-creating the table with the model info and finally restore its data back
            User::tableReset();
            
            //OR
            
            // Reset ManyTable at once with
            Db1::tableResetAll( User::class, Blog::class,  Comment::class);
    
            
    
            // Create ManyTable With
            Db1::tableCreate( User::class, Blog::class,  Comment::class);
            
            // Create many at once
            Db1::tableCreateAll( User::class, Blog::class,  Comment::class);
    
            
            // Save Single Backup
            NewsLetterSubscriber::tableSaveBackup(path_asset('backups').'/NewsLetterSubscriber.model.json')
           
           // Load Single Backup
           NewsLetterSubscriber::tableLoadBackup(path_asset('backups').'/NewsLetterSubscriber.model.json')
        
            // Save all model
            Db1::tableSaveBackupAll(...),
          
            // Load all model backup
            Db1::tableLoadBackupAll(...)
    
    
    
            // generate demo data
            // This will generate 10 data row and add theme to table
            User::insertMany( User::generateDemoData(10) );
            
            // OR customized with
            
            $userInfo = ['user_name'=>'samtax', 'password'=>'1234', 'email'=>'samtax01@gmail.com', 'role'=>(new User())->role ];
            $userAdmin = ['user_name'=>'samtax_admin', 'password'=>'1234', 'email'=>'samsoniyanu@hotmail.com', 'role'=>'admin'];
            User::insert(User::generateDemoData(1,null, null, $userInfo)[0], ['user_name', 'email']);
            User::insert(User::generateDemoData(1,null, null, $userAdmin)[0], ['user_name', 'email']);
        }
 ```
 
 
 ## Database Model and Table information
 * ```app_db_table_list()``` or ```Db1::getExistingTables(true)``` :  Get all table list, i.e Existing Model and Table List
 * ```app_db_model_list()``` or ```Db1::getExistingModels(true)``` :  Get all available model and there table name, i.e Existing Model with table List

 
 
 
  ```NOTE``` Before Use can Perform any of this Functions, The Model Class must Extend Model1 abstract Class
    
  * Example, A Simple Blog Model Class Could Be
  ```php
    class Blog extends Model1 {
    
        public $id = -1;
        public $title = '';
        public $slug = '';
        public $body = null;
        public $posted_at = null;
        
     }
  ```
  
  ```The Above Model Would Generate Equivalent SQL of```
 
  ```sql
  
    CREATE TABLE IF NOT EXISTS `blogs` (`id` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT, PRIMARY KEY(`id`),`title`  VARCHAR(250) COLLATE utf8mb4_unicode_ci NULL DEFAULT '' , `slug`  VARCHAR(250) COLLATE utf8mb4_unicode_ci NULL DEFAULT '' , `body`  TEXT COLLATE utf8mb4_unicode_ci NULL DEFAULT NULL , `posted_at`  TIMESTAMP NULL DEFAULT NULL , `created_at`  TIMESTAMP NULL DEFAULT NULL , `updated_at`  TIMESTAMP NULL DEFAULT NULL ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
         
  ```
  
  
 ## How It Works
 Php Typeless Variable is still enough to generate our SQL dataType.
 ```*
  Sql DataType are assigned Using Php Variable Value DataType (either String, Integer, Boolean, or null) only if The Variable name does not contain
 any prefix keyword like "_at", "_date" and "_time"
 
 ```SQL DATATYPE ASSIGNMENT LOGIN```
 * ```DATETIME :``` Variable Name must ends with "_at", E.g created_at, updated_at
 * ```DATE :``` Variable Name must ends with "_date", E.g birth_date, vacation_date
 * ```STRING :``` Variable Value must equal to String E.g ('', or "", or "Default Value")
 * ```TEXT :``` Variable Value must equal to NULL E.g null
 * ```INTEGER :``` Variable Value must equal to Number E.g (0, 1 or any integer)
 * ```BOOLEAN :``` Variable Value must equal to Boolean E.g (true or false)
 * ```ENUM :``` Variable Value must equal to ARRAY E.g []
 

  
  
  
## User Class Model extends AuthModel1 Instead
  For Additional Features and Functions like Login(), getLogin(), register(), getAvatar(), isLoginExist() e.t.c
   User Model Extends AuthLogin. 
  
  * Example, A Simple User Model Class Could Be
  ```php
    class User extends AuthModel1 {
    
        public $user_name = '';
        public $full_name = '';
        public $email = '';
        public $sex = '';
        public $password = '';
        public $description = null;
        
     }
     
     
  ```
  
  ```The Above Model Would Generate Equivalent SQL of```
  ```mysql
    CREATE TABLE IF NOT EXISTS `users` (`id` BIGINT UNSIGNED NOT NULL AUTO_INCREMENT, PRIMARY KEY(`id`), `user_name`  VARCHAR(250) COLLATE utf8mb4_unicode_ci NULL DEFAULT '' , `full_name`  VARCHAR(250) COLLATE utf8mb4_unicode_ci NULL DEFAULT '' , `email`  VARCHAR(250) COLLATE utf8mb4_unicode_ci NULL DEFAULT '' , `sex`  VARCHAR(250) COLLATE utf8mb4_unicode_ci NULL DEFAULT '' , `password`  VARCHAR(250) COLLATE utf8mb4_unicode_ci NULL DEFAULT '' , `description`  TEXT COLLATE utf8mb4_unicode_ci NULL DEFAULT NULL , `created_at`  TIMESTAMP NULL DEFAULT NULL , `updated_at`  TIMESTAMP NULL DEFAULT NULL , `last_login_at`  TIMESTAMP NULL DEFAULT NULL ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
  ```
  
  

## Manual SQL Property Set
Model property can be set manually in two ways
1) in model
```php
  class User extends AuthModel1 {
  
     public static $COLUMN_SQL_CREATE_PROPERTY = [
         'user_name'=> "VARCHAR(250) COLLATE utf8mb4_unicode_ci NULL DEFAULT ''",
         '`$password`'=> "VARCHAR(100) COLLATE utf8mb4_unicode_ci NULL DEFAULT ''",
         '`about`'=> 'TEXT', 
     ]
  
     
      public $user_name = '';
      public $password = '';
      public $about= '';
      
   }
```

1) Or when Creating a table, Usually in debug function
```php
    User::tableCreate($columnSetSQLProperty = [
            'user_name'=> "VARCHAR(250) COLLATE utf8mb4_unicode_ci NULL DEFAULT ''",
             '`$password`'=> "VARCHAR(100) COLLATE utf8mb4_unicode_ci NULL DEFAULT ''",
             '`about`'=> 'TEXT', 
    ])
```
  
  
  
  
## Ehex Model1 Class
    
- ```Db1``` : (create Database and exec query on table). 
        Db1::exec('Select * from '.User::getTableName());
    
- ```Model1``` : Consist of query function for model, and is created in Database with the help of php object variable type. 
     Variable type like 
 
    *class User extends Model1 {*
    
    - *$full_name = ""* generate "varchar" in Database because string value
    
    - *$description = null* generate "text" in Database because of null value
    
    - *$age = 0* generate "integer" in Database because of integer value.
        
    - *created_at* generate "timestamp" in Database because of the *_at* ending the variable.
    
    - *created_date* generate "date" in Database because of the *_date* ending the variable.
    
    - *created_time* generate "time" in Database because of the *_date* ending the variable.
    
    - And You can also render the entire field with ```$user::render()``` OR ```$user::renderForm()``` to render with update form
    
    
### Model Method

- Using [ User ] Model as Example, You can  Access methods like 
    
- User::selectAll(): get All User Information.
    
- User::find($user_id = 1): get User with primary id 1 Information.
    
- User::updateMany(['age' => 20, 'user_name' => 'samtax01']): get User with primary id 1 Information.
    
- User::xcrud() : A very cool instance of Xcrud Data Management. e.g echo User::xcrud()->render().
    
- User::getTableName() :  Get Database table name. ( Overridable in Model Instance Class )
    
- User::query() : Query Builder through User::query()->where(...) similar to Laravel Eloquent Builder, 
    
- User::setField($id, $fieldName, $fieldValue) : Update Model database single column value
    
- User::getField($id, $fieldName, $defaultValue = null) : Get Model database single column value
    
    
        
        
        
        - Use User::help() to get Other Helping methods...
    
        
- ```AuthModel1``` Extended version of Model for Auth Model like User should use *AuthModel1* instead of *Model1*
      
   ```php
        class User extends AuthModel1 {
        
            then use can access Auth information like 
            
            // Auth Information
            - $user->register($request = $_REQUEST,  $uniqueColumn = ['email', 'user_name'])  // create account from request data or manually input array of fields and values 
           
            - $user->login($user_name_or_email = 'samtax01', $password = '1234', $search_in_likely_column_name = ['email', 'user_name'], $search_in_likely_column_password =  ['password'])
            
            - $user = User::getLogin(); // After login, the you can get user information in any page just by calling *User::getLogin()*, Example ```User::getLogin()->full_name``` OR ```User::getLogin()['full_name']```
            
            - $user->re_login() : use to refresh login cache expecially when updating information
            
            - $user->isLoginExist() : check if user as login
            
            - $user->logout($redirectTo = '/') : delete account session cache
            
            - $user->delete() : delete account from Database
            
        
            // Upload Avatar
            - $user->uploadAvatar($file_path = $_FILES['avatar']['tmp_name']);
            
            - $user->getAvatar()
            
            
            // Upload File    
            - $user->uploadFile($file_path = $_FILES['other_picture']['tmp_name'])
            
            - $user->deleteFile($file_name = '')
            
            - $user->getFileUrl($file_name)
            
            
            // Main Upload File    
            - $user->uploadMainFile($file_path = $_FILES['avatar']['tmp_name']) // to upload in Main User Folder. (folder 1)
            
            - $user->deleteMainFile($file_name = '')    // to delete from Main User Folder. (folder 1)
            
            - $user->getMainFileUrl($file_name) // to get file from Main User Folder. (folder 1)
            
    ``` 
    
    
    
    
      
## Ehex Model1FileLocator Class
Model1FileLocator keeps website asset in one place. it is like a media finder or a media like database. It has being integrated with Model1 and can be access via
model upload method. ```$model1->uploadFile($_FILES['image']['tmp_name'], $file_name = '', $addUrlToModel1FileLocator  = true);```,
just enable $addUrlToModel1FileLocator third parameter.
### Usefulness of Model1FileLocator
1. Put all assets in one database table
1. Easy Access to resource for API
1. Easy Access to resource for Multi-Website
1. And More...
Because it might seems hard to retrieve model assets Files from another website or in Api, Therefore Model1FileLocator  
solve this by saving assets data url in database. It Can also retrieve it just by calling  ```Model1FileLocator::getFromDb()```
### Model1FileLocator Function
- ```Model1FileLocator``` : Model File Url Saver, Save and Retrieve File From Model with Model Name and Model Unique Id.

> Save
- ```insertAll_fromFile_toDb($model, $append = false)``` : Upload Model File System Asset to server database
- ```insertUrl($model, $file_url = null, $file_name = null, $tag = null, $other_url = null)``` : Just Save File Information to DB
- ```uploadFiles_andInsertUrl($model, $fileRequest, $append = true)``` : Upload File and Save File Information to DB

> Get

- ```find($model = null, $file_name = null, $orDemoPictureUrl = null)``` : Get File from File and DataBase
- ```find_inFile($model = null, $file_name = null, $orDemoPictureUrl = '...')``` : Search File
- ```find_inDb($model = null, $file_name = null, $urlOnly = true)``` : Search Db

> Select All
- ```selectAll($model = null)``` : Get All from File and DataBase
- ```selectAll_fromDb($model, $urlOnly = true, $asObject = false)``` : 
- ```selectAll_fromFile($model, $extension = [], $recursive = false)``` : 

> Delete
- ```deleteAll($model = null)``` : 
- ```delete($model = null, $file_name = null)``` : 
- ```delete_fromFile($model, $file_name = null)``` : 
- ```delete_fromDb($model, $file_name = null)``` : 
- ```delete_fromDb_byFieldName($uniqueField = -1, $columnName = 'id')``` : 

Structure of Model1FileLocator
```php
    class Model1FileLocator extends Model1{
        public $id = 0;
        public $model_name = '';
        public $model_id = 0;
    
        public $name = null;
        public $url = null;
        ...
```
    
    
   
 ## SQL Column Value Filtered
 for security reason, all value of column are being filtered automatically, you can ignore any column with static variable $COLUMN_NO_FILTER_LIST,
 ``` public static $COLUMN_NO_FILTER_LIST = ['columnName1', 'columnName2''];```
 or set ```$FLAG_COLUMN_NO_FILTER = true``` to ignore filter for model
 
 To Filter column and restore it on fetched, use ```$COLUMN_UN_FILTER_LIST```, This can be useful in blog and content with markup format.
 For Example.
 Example in  blog, you don't want blog markup content to get filtered away or replaced. Therefore you can include ```public static $COLUMN_UN_FILTER_LIST = ['columnName'];``` 
 in your model class
 ```php
     class Blog extends Model1 implements Model1ActionInterface {
         public static $COLUMN_UN_FILTER_LIST = ['body'];
     
         public $id = 0;
         public $name = '';
         public $slug = '';
         public $body = null;
         
 ```
 This can be done manually with either ```MySql1::mysql_unreal_escape( $value )``` or ```MySql1::mysql_unreal_escape_lite( $value )```.
 You can also filter many field at the same time with ```$model = MySql1::unFilterValueIfKeyExist($model, ['body', 'description'])```
 
 
 
 ## Session Database [ DbPref1 ]
 DbPref1 ( database preference ) is an inbuilt Model that can be used to store data on the go. Usually key value data.
 it save and get value just like  session / cookie. This is a plan to save string value, Object, Model, in database
 e.g Model Extra data,Session Data. or any simple data.
 
 Create Model Table if not exists.
 ```DbPref1::tableCreate()```
 ```php
     // field 
     public $id = -1;
     public $user_id = "";
     public $key = "";
     public $value = null;
     public $name = "";
     
     // get queryable Model From dbPref
     DbPref1::toModel();
     
     // create Model Class
     DbPref1::tableCreate();
     
     // Clear Model
     DbPref1::tableTruncate();
     
     // Reset Model
     DbPref1::tableReset();
 ```
 
 Save Model Instance Data
 
 ```php
     // Save data with respect to Model Instance
     save_modelData($model, $keyName = '', $value='');
     
     // Get data with respect to Model Instance
     get_modelData($model, $keyName = '');
     
     // Delete data with respect to Model Instance
     delete_modelData($model, $keyName = '');
```

Example, key value data
```php
    DbPref1::modelSave(User::withId(1), 'name', 'Samson Iyanu');
```
or, key and multiple data
```php
    DbPref1::modelSave(User::withId(1), 'color', ['blue', 'yellow']);
```
or, key and key Value array
```php
    DbPref1::modelSave(User::withId(1), 'name', ['full_name'=>'samson iyanu', 'user_name'=>'samtax01']);
   // access with
   echo DbPref1::get_modelData(User::withId(1), 'name', false)['full']
```

>  Insert New / Append / Override Existing data ```save($name, $object_or_keyValueArray = null,  $user_id = '', $replace = true)```

> Get  Data ```get($name, $user_id = '', $convertToClassName = null)```

> Get Likely  Data Using With. use $nameFormat '%{data}%' for like data, or single % for either left or right ```getMany($nameLike = '_', $user_id = '', $nameFormat = '%{data}%')```

> Get If Name Start With Data ```getManyIfStartWith($namePrefix = '_', $user_id = '')```

> Get If Name Contain ```getManyIfContain($name = '_', $user_id = '')```

> Get If Name End With Data ```getManyIfEndWith($nameSuffix = '_', $user_id = '')```

> Get All Row ```getRawRows($where = ['name'=>'', 'user_id'=>''],  $login = ' AND ', $operator = ' = ' )```

> Get Associated User Info ```getByUser($user_id = '')```

> Is data Exists ```exists($name, $user_id = '')```



## Pagination
Pagination can be used in query builder with 
```php
    // init 
    $paginatedData = User::query()->where('sex', '=', 'male')->paginate(10);
    
    // Get All Fetched Data
    foreach ($pagination as $row){
        echo $row->id;
    }
    
    // render
    echo $paginatedData;
``` 


But Since Model1 Default Query is More or less to Pure Sql Merged together,  Pagination can be use from any selected Model1 instance.
Just Put your prefix query in paginationLimit($query='...'), method of Model.
E.g
```php
    // Init Pagination From your Query
    $pagination = User::selectMany( true, User::paginationLimit('WHERE sex = "male"', 10) );
    
    // Get All Fetched Data
    foreach ($pagination as $row){
        echo $row->id;
    }
    
    // Render Pagination Template with
    echo User::paginationRender();
```


Or If you want pagination for your pure sql query ```$paginate = MySql1::makeLimitQueryAndPagination($prefixQuery = '', $total = 0, $limit = 10, $templateClass = BootstrapPaginationTemplate::class, $requestPageKeyName = 'page')```
```php
    $paginate = MySql1::makeLimitQueryAndPagination($prefixQuery = 'SELECT * FROM users', $total = 'SELECT count(id) FROM users', $limit = 10, BootstrapPaginationTemplate::class, $requestPageKeyName = 'page')
    
    // Get Query
    echo $paginate->query; // Output is SELECT * FROM users limit [1], 10
    
    // Render Pagination
    echo $paginate->paginate;
```

And you can simply Use ```MySql1::makeLimitQuery($page = 1, $limit = 10)``` for your API instead.
        

 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
