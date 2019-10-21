
# Introduction
Since Ehex comes with Some popular models like Blog, NewsLetter, Inbox, Testimony and so on... for easy use, 
We added a dynamic dashboard which can work with any model or controller that inherited ```Model1ActionInterface``` Interface and implement the lifecycle functions correctly according to the below example.

## Dashboard Model Example
Example of Such Model is 
```php

class ContactUs extends Model1 implements Model1ActionInterface {

    public $id = 0;
    public $full_name = '';
    public $phone_number = '';
    public $description = null;
    public $email = '';
    public $is_active = true;


    /* Save to database  */
    static function processSave($id=null){
        $result = self::insertOrUpdate(request(['is_active']));
        Session1::setStatusFrom('Message Sent', $result->getMessage());
    }
    
    
    /* CRUD Table Management for data update/search/delete */
    static function manage(){
        return self::xcrud()
            ->columns('updated_at, created_at, id', true)
            ->fields('created_at, updated_at', true)
            ->unset_title();
    }
    
    
    /* Dashboard Analytic Widget Model Manage */
    static function getDashboard(){
        return [
            ['name'=>'All Message', 'value'=>Message::count(), 'linkName'=>'View All', 'linkUrl'=>'/message/manage'],
            ['name'=>'Active Post', 'value'=>Message::count('where is_active = true'), 'linkName'=>'View All', 'linkUrl'=>'/message/manage'],
        ];
    }
    
    
    /* Add to Sidebar Menu */
    static function getMenuList(){
        return [
            'Contact Us Message'=>[
                url('/contact/manage')=>'<i class="fa fa-message"></i><span> Contact Us Messages</span>',
                url('/contact')=>'<i class="fa fa-plus"></i><span> Contact Us</span>',
            ]
        ];
    }
    
    
    /* Model Route List */
    static function onRoute($route){
        // all user
        $route->view('/contact', 'pages.common.contact.index');
        
        // only admin
        if(Auth1::isAdmin()){
            $route->view('/contact/manage', 'pages.common.contact.admin.manage');
        }
    }       
    
    ...
```

To get all model that inherited Model1ActionInterface, Simply call ```app_class_with_interface(Model1ActionInterface::class)```, which will return array of class that inherited that.
This is used in Dashboard::getMenuList() to pie all others model getMenuList() which is used to create sidebar menu for the dashboard template automatically.



## Auto Manage Model
Some information, or Plugins  Data needs to be manage without having to create a view for them. Therefore, Dashboard template as a default view for managing Model or Class that has manage() static method and must have render() method as well.
This View is located in ```resources/views/layouts/shards_dashboard/pages/manage.blade.php``` and can only be accessible by admin role (edit and change role if you want), and it is can be visited with ```url('dashboard/manage')``` but it requres Your Model or Class parameter. i.e to manage User Model,  ```url('dashboard/manage?model=User')```. Voila!.

A more simpler way to access this is to call Dashboard  ```Dashboard::getManageUrl(User::class)``` which is still equal to ```url('dashboard/manage?model=User')```, but offers a neater way of accessing it.

> Example is 
```php
    class PaymentTableData extends Model1 implements Model1ActionInterface{
    
        ...
        // Method to be rendered
        static function manage() {
            // This method can initially contain html string 
            // but must return a renderable method or null. e.g
            echo '<h3> Other Data Can Stay Here... </h3>';
            return PaymentTableData::xcrud();
            
            
            // To make this easier, you can user the Html1 class which make 
            // use of constructor or append() and render() method too. e.g
            return new Html1(function(){
                echo '<h3> Other Data Can Stay Here... </h3>';
                echo PaymentTableData::xcrud()->render();
            });
            
        }
    
    
        // this method add menu to sidebar
        static function getMenuList() {
            return [
                'PayStack'=>[  
                    Dashboard::getManageUrl(PaymentTableData::class) =>'<i class="fa fa-gear"></i><span> Payment Data </span>', 
                ],
            ];
        }
        ...

    }
```


> Run through  ```exPaystackPaymentGateway{ }```  code as  example
