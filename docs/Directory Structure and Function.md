# Directory Structure and Functions


## Folder Structure
Too Long or Scattered folder structure can look so overwhelming, Therefore, We concentrate on keeping the folder structure very simple and sensible.
We Only Have There Main Folder, **app** (for models, api and controllers), **Resources** (for Assets and Pages) and **Includes** (for Ehex and Third Parties Libraries) 

## Folder Structure
    Project Name
        app
         |_ api
         |_ controllers
         |_ models
         
        
        includes (optional: because it can also be placed outside for multiple project)
            |_ __Php : This consist of the built in functions that make the framework works
            |_ __PhpLibrary : This consist of Third Party Library
            |_ __Config : This consist Library Configuration for ehex... 
            |_ __Shared : Common Resources shared by all project
                
            
        resources
           |_ assets : common assets for all themes
           |_ cache : This consist of page settings, blade view e.t.c cached
           |_ plugins : download or create once for additional features
           |_ views : interface informtion
               |_ layouts : a.k.a theme folder
               |_ pages : all pages for the project. And They extend Layout
        
        
        
Deep Explanation On Some of the Folders

    __Php  
        Consist of
        Ehex.php : the base library which others library depends on.
        EasyForm.php: Consist of Form Creating function
        EasyDb.php : Consist of Db1 (__FILE_BASE mysqli), Model1 which all model must extend 
            
            
        Views
           This consist of your created page ( Blade Views ).
           Example is resources/views/user/dashboard.blade.php,
           which will be reference with user.dashboard in ROUTE and other VIEW
           
           
           
         

          
          
          
          
## Common Directory Functions
This function are required to navigate through the code. They provide path address for each needed location.


## Directory Functions
 1. ```path_main()``` : get the main project path e.g /Applications/MAMP/htdocs/ProjectName/
 1. ```path_main_url()``` : get the website url main path. e.g http://localhost/ProjectName/
 1. ```path_asset()``` : get the full file path of assets and the *path_asset_url()* for url form of assets path
 1. ```path_asset_url()``` OR asset() : get the current project assets path file url located in *ProjectName->resources->assets*
 1. ```path_current()``` : Get Current Path
 1. ```path_current_url()``` : Get Current Path Url
 1. ```path_url_exists_or($url = '', $default = '...')```   or Url1::pathUrlExistsOr(...) : Verify if url exists or use default




 ## App Path
 1. ```path_app()``` : get the project app directory e.g ProjectName/app/
 
 

 ## Shared
 1. ```path_shared()```           : get shared directory path  *includes->shared*
 1. ```path_shared_resources()``` : get shared directory resources path. *includes->shared->resources*
 1. ```path_shared_app()```       : get the project app directory *includes->shared->app*
 1. ```path_shared_asset()```     : return system path to assets of includes (this is available for all project using the same includes folder). e.g *http://localhost/includes/shared/resources/assets*
 1. ```path_shared_asset_url()``` OR shared_asset() : return url path to assets of includes (this is available for all project using the same includes folder). e.g  */Applications/MAMP/htdocs/includes/shared/resources/assets*

## Resources Path
1. ```resources_path()``` : return  current project resources path
1. ```resources_path_view()``` : return  current project view path
1. ```resources_path_cache()``` : return current project cache path
1. ```resources_path_view_cache()``` : return current project view cache path
1. ```resources_path_asset()```  : return current project assets asset path
1. ```get_valid_view_path()```  : Automatic lookup for view in either current app view folder or shared view folder. return null if folder not exist


 ## Assets Path are
 1. ```asset()``` OR ```path_asset_url()``` : get the current project assets  file path url located in *ProjectName->resources->assets*
 1. ```layout_asset()``` : get the current layout theme in use, when you extend layout in your view, works with ```register_path_for_layout_asset()``` or specify your layout_name manually file path url in *ProjectName->resources->view->layouts->themeName*
 1. ```current_layout_asset()``` : get current file layout assets, Current Layout Assets function Can only be run in Layout Folder, to retrieve assets of the layout. E.g Can be use to retrieve layouts/plugin_name/assets/css for plugin instead of using layout_asset("layout-name") *ProjectName->resources->view->layouts->themeName*
 1. ```shared_asset()``` OR ```path_shared_asset_url()``` : this is available for all project using the same includes folder, file path url located in *includes->shared->resources->assets*
 
 
 
## Helper Function
1. ```redirect(...)``` OR ```exUrl1::redirect(...)``` use to navigate through route
1. ```redirect_back(...)``` use to navigate to previous route
1. ```redirect_failed(...)``` use when error occurred to navigate away from current route to a default safe route (could be backward or error404 page), where can print error 
    e. g
    ```php
        if(Auth1::isGuest()){
           redirect_failed(['User Failed', 'Permission Denied!', 'error'])
        }
    ```
    
