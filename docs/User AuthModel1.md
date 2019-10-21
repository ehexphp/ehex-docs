
# AuthModel1
This is an Optimized Model1 class for Saving application user authentication data. User Class must extends ```AuthModel1``` instead of ```Model1```.

## Auth Form
We have login, register, forgot_password, reset_password and edit_profile Form page with inbuilt User class method 
that can handle all the process requested, all located in resources/views/pages/auth folder.
with default route located in config/route.php method ```make_default_route()```. Which is called in Config >> onRoute() method ```make_default_route('/dashboard');```
accept /dashboard route name.
1. url('/forgot_password')
1. url('/reset_password')
1. url('/register')      
1. url('/login')         
1. url('/logout')     
1. url('/delete_account')

> Example methods are
```php
    class User extends AuthModel1{
       // Important Field
        public $id = 0;
        public $user_name = '';
        public $email = '';
        public $full_name= '';
        public $role = 'user';
        public $password = '';


        /**
         * Login Existing User
         */
        static function processLogin(){
            $result = User::login($_REQUEST['user_name'], $_REQUEST['password'], ['user_name', 'email'], ['password']);
            Url1::redirectIf(Session1::getLastAuthUrl(true, routes()->dashboard), '', $result);    // on success redirect
            Session1::setStatus('Failed', $result->getMessage(), 'error'); // on error, set status
        }
        
        
        /**
         * Register New User
         */
        static function processRegister(){
            // Register
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

## Auth Login and Account Information
User login method is ```login($user_name_or_email, $password, $search_in_likely_column_name = ['email', 'user_name'], $search_in_likely_column_password =  ['password'])``` 
Example Login is.  ```User::login($_REQUEST['user_name'], $_REQUEST['password'], ['user_name', 'email'], ['password']);```.
It will  Save Login Information to Session Automatically. to get login from any page Use ```User::getLogin()``` or 
```User::getAllowedRoleLogin(['user']);``` and   ```User::isGuest()```  to know if is guest(i.e no login exists) or ```User::isAdmin(...)``` otherwise.

## Auth Register
User register method is ```register($request = null,  $uniqueColumn = ['email', 'user_name'])```
Example Usage is  ```User::register($_REQUEST, ['email', 'user_name']);```.




  
 
## Role Permission
Some pages are built for specific user type and the user role list are usually in hierarchical order. 
Therefore, we created a helping method to help select list of role from the beginning to given role.

> Example of a User AuthModel1 and it Role Permission Could be
```php
    class User extends AuthModel1{
       // Important Field
        public $id = 0;
        public $user_name = '';
        public $email = '';
        public $full_name= '';
        public $role = 'user';
        public $password = '';
        
        // Available Role List, in Descending order
        static function getRoles(){ 
            return ['admin', 'staff', 'user']; 
        }
        
        // get allowed role access list
        static function getRolesFrom($role = ''){ 
            return Array1::splitAndGetFirstList(self::getRoles(), $role);
        }
    ...
```

For Example.
1. in this list of role ```['admin', 'staff', 'editor',  'user']```, if you supply ```admin``` then admin can definitely access all sub roles.
So we are expecting **admin, editor, staff and user**. 
```dd( User::getRolesFrom('admin')  )``` will return admin and all sub role.

1. Example 2, in role list of ```['admin',  'staff', 'editor',  'user']```, if you supply ```staff``` then staff can access only **staff, editor, user**.
While **editor** role. will only access only **editor and user** role
```dd( User::getRolesFrom('staff')  )```

To Make this possible, We created  three methods  ```getRoles()``` ,  ```getRolesFrom(...)```  and   ```isRoleWithin(...)```
```php
  class User extends AuthModel1{
        ...
        public $role = 'user';  // your default role after a new registration

        // Should Contain Available Role List, in Descending order
        static function getRoles(){  
            return ['developer', 'admin', 'staff', 'user'];
        }
        
        // get allowed role access list
        static function getRolesFrom($role = ''){ 
            return Array1::splitAndGetFirstList(self::getRoles(), $role);
        }
        
        // Confirm if logged in user role exists within First Row and Specified $toWhichRole End Role... e.g from admin to staff
        static function isRoleWithin($role, $toWhichRole = 'staff'){ 
            return Array1::contain(Array1::splitAndGetFirstList(User::getRoles(), $toWhichRole), $role);
        }
        ...
```


Example using  ```isRoleWithin(...)``` to know if user role is within admin to staff. this will allow admin or staff to see additional menu option *Admin Dashboard*
```php

    // Addtional Menu List if Admin or Staff

    <li> <a href="{{ url('/account') }}" title="">Account</a> </li>

    @if(User::isRoleWithin(User::getLogin()->role, 'staff'))
        <li><a href="{{ url('/admin') }}">Admin Dashboard</a></li>
    @endif
    
    
     * You can also use it as
     *  e.g  User::isRoleWithin($userInfo->role, 'staff'); // which check if user is within the role.
     *      Or Simply
     *         User::isRoleWithin(['admin', 'manager', 'staff']) // or
     *         User::isRoleWithin( User::getRolesFrom('staff') )

```





## Validate and Get Login 
To validate user authentication and get User Model Information for logged in user, Use User::getLogin(...) or User::getAllowedRoleLogin(...). But Instead of 
```User::getAllowedRoleLogin( ['staff', 'user']  )```  which will allowed only staff and user access, you can use ```User::getAllowedRoleLogin( User::getRolesFrom('staff')  )```
This will use user role column to validate User Permission level. Put at the top of every  pages you want permission level.

Usually dashboard  or user profile page requires permission at the top of the page. if User has not Signed in, or Signed in User permission is low, 
User::getAllowedRoleLogin( ['staff', 'user']  ) will redirect User to login page for proper authetication.

 
 
 
 
 
 
 
 
 
 
 
 
 
