# Starting new project
 five simple step to create your project

## Step 1) To Create a new project with Ehex.
Clone the Ehex Repository.

Open the **".config.php"** and input your database  information like valid user and password. Goto to debug and run ```Db1::databaseCreate()```. *ensure  config DEBUG_MODE = true*

## Config Settings
```php
class Config1{
    const DB_DRIVER = 'mysql'; // or 'sqlite'
    const DB_HOST= 'localhost';
    const DB_NAME= 'project_name';
    const DB_USER = 'root';
    const DB_PASSWORD= '';
    
    static function onDebug(){
        //run database create once and comment it out
        Db1::databaseCreate();
    }
```

## Step 2) Create Model for your app.
goto app/model folder, and create any class you like,e.g  create Product.class. Declare the class and make it extend Model1 class.
```php
    class Product extends Model1{
        public $id = 0;
        public $name = '';          
        public $price = 0;
        public $description = null;
    
    }
```
Go back to your ".config.php" Config1 class, and onDebug() method, run  ```Product::tableCreate()```.   *ensure config DEBUG_MODE = true* 
```
class Config1{
    static function onDebug(){
        
        // add this to create table automatically for Product model, with the model variable names
        Product::tableCreate();
        ...
    }
}
```


## Step 3) Create Add View.
View is always required to manage and display Model Data. Create a new php page (view_name.blade.php) in resources/views/pages. *You can put that under another folder to group your view for readability sake*.
```
    //  file resources/views/pages/add_product.blade.php
    
    <form action="{{ Form1::callController(Product::class, "processSave()") }}">
        {!! form_token() !!}
        echo Product::makeForm()->render();
        <button type="submit"> Save Product </button>
    </form>
```

## Add Method processSave() to your Product Class
```php
    class Product extends Model1{
    
        ...
        static function processSave($id = null){
        
            // insert or update
            $result = self::insertOrUpdate(request());
            if($result && $result->id > 0){
                redirect('blog/'.$result->id.'/edit', ['Saved', $result->getMessage(), 'success']); 
            }
            
        }
        ...
        
    }
```
So when you click save product, Our Data get Saved in Database products table.
> For more help on ex form rendering, Visit [Form](Form.md) and  [Html Generator](Html Generator.md)


## Step 4) Create Manage View.
Display all Model data in Table. Create a new php page (view_name.blade.php) in resources/views/pages. *You can put that under another folder to group your view for readability sake*.
```
    // file resources/views/pages/manage_product.blade.php
    echo Product::xcrud()->render();
```
> For more help on ex form rendering, Visit [Form](Form.md) and  [Html Generator](Html Generator.md)



## Step 5) Add page to route 
```php
    class Config1{
    
        ...
        static function onRoute($route) {
            $route->view('/add_product', 'pages.add_product');
            $route->view('/manage_product', 'pages.manage_product');
        }
        ...
        
    }

```


> Visit your url with *http://samplesite.com/add_product* and *http://samplesite.com/manage_product*

> Please check out our official sample project. It will only take few minute but you will definitely understand better 
