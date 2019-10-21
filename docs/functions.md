# Functions
> See [Check full api function](https://ehex.github.io/ehex-docs/api/docs/index.html ':include :type=iframe width=100% height=800px')
          
## Functions
Ehex Functions




## Directory Functions
 1. ```path_main()``` : get the main project path e.g /Applications/MAMP/htdocs/ProjectName/
 1. ```path_main_url()``` : get the website url main path. e.g http://localhost/ProjectName/
 1. ```path_asset()``` : get the full file path of assets and the *path_asset_url()* for url form of assets path
 1. ```path_asset_url()``` OR asset() : get the current project assets path file url located in *ProjectName->resources->assets*
 1. ```path_current()``` : Get Current Path
 1. ```path_current_url()``` : Get Current Path Url
 1. ```path_url_exists_or($url = '', $default = '...')```   or Url1::pathUrlExistsOr(...) : Verify if url exists or 

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
    
    
    
```
    __	
    api_and_form_default_route	
    app	Get App Route and RequestInformation
    app_api_list	get list of api class
    app_class_list	
    all known app class. e.g AuthModel1::class, Model1::class, Controller1::class, Api1::class;
    
    app_class_paths	app full class path list
    app_class_with_interface	
    app_controller_list	app controller list
    app_dashboard_list	get model with Dashboard Interface
    app_db_model_list	Existing Model with table List
    app_db_table_list	Get Existing Model and Table List
    app_model_list	get all model list
    app_page_list	
    get model with page interface. E.g FrontendPage::class, DashboardPage::class
    
    asset	Get Assets files
    csrf_token	Get Session Token
    current_layout_asset	
    Use in your layout to get current location of layout assets Get Current Layout Assets ( Function can only be used inside resources/layouts/layouts_name )
    
    current_plugin_asset	
    Use in your plugin to get current location of plugin assets Get Current Plugin Assets ( Function can only be used inside resources/plugins/plugin_name )
    
    current_resources_asset_path	Use in your plugin/layout to get current location of assets
    d	Use for debugging but don't end page
    dd	Use for debugging and end page
    file_base	
    file_session	
    file_session_get	
    file_session_remove	
    file_session_save	
    form_call_api	Generate Url to API
    form_call_controller	Generate Url to Form Controller
    form_token	Get Session Token Form, Hidden form with token field
    get_all_view_in_directory	
    get_valid_view_path	Automatic lookup for view in either app view folder or shared view folder. return null if folder not exist
    is_token_valid	validate token
    is_ajax_request	
    layout_asset	Get Layout Assets directory ( Layout serve as Theme )
    mailer	
    mailer_send_mail_to_list	
    make_default_route	
    makeRoute	
    normalizeSharedPath	
    if shared_assets, create symlink to assets and put under website. (otherwise move __include folder to your website) because assets file must be a subdirectory under website domain and not outside website folder wic we don't knw url location for. normalizeSharedPath('', '/shared/resources/plugins/', 'plugin_asset');
    
    now	return current date and time information
    now_date	Current date information
    now_time	current time information
    old	All Last Request, Select From Last Request of No Parameter to return all.
    paginate	
    path_app	
    path_asset	
    path_asset_url	
    path_clear_cache	Delete All Ehex Creatd Cache
    path_main	
    path_main_url	
    path_shared	Shared Path Information
    path_shared_app	
    path_shared_asset	
    path_shared_asset_url	
    path_shared_resources	
    path_to_viewpath	
    plugin_asset	Get Plugin Assets directory
    pre	
    redirect	
    Goto a particular address use mostly in controller
    
    redirect_back	Goto a previous address
    redirect_failed	redirect to previous page or to error404 Page if previous failed
    redirect_to_view	use mostly in controller
    register_path_for_layout_asset	Call on all layout template. It will use the template path to locate assets folder and use it for the website
    request	
    Get any sent request like $_GET, $_POST and $_FILES... Also all value are being normalized. Checkbox value are set to either true of false, and files are well arrange, in Parameter, Set the Names
    
    resources_path	
    resources_path_asset	
    resources_path_cache	
    resources_path_plugin	
    resources_path_view	
    resources_path_view_cache	
    resources_path_view_layout	
    route	redirect with route | Navigate to any menu
    routes	
    Navigate to any menu and Return all Menu, can be accessed through routes()->login =======[ get all route link and value array as ArrayObject and call by name, e.g routes()->home, routes()->current, ]=================
    
    shared_asset	
    token	Ehex Token Generator
    translate_language	
    translated_language	
    url	redirect with route url
    view	
    view_exists	
    view_make	
    viewpath_to_path	Get View full path
```
    


> Also See [Directory Structure Function](https://ehex.github.io/ehex-docs/Directory%20Structure%20and%20Function.md ':include')
