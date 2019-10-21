
# Introduction
Plugin provides Re-usability of Code written by you or other developers. Plugin is like code extension provides you with more ready-made features allows you to concentrate on other path of your code application, Create or Download plugins to your ```resources/plugins``` folder

## Plugin
Ehex has rich set of libraries, which you can find in __includes/ folder but Ehex is not just limited to inbuilt library, you can create your own libraries too, which can be stored in resource/plugins folder. You can create libraries in three ways.
1) Create new library
1) Extend the native library
1) Replace the native library

## Create New Library
While creating new library one should keep in mind, the following things −
1) The name of the file must start with a capital letter e.g. Mylibrary.php
1) The class name must start with a capital letter e.g. class Mylibrary
1) The name of the class and name of the file must match.


## Ehex Plugin
Ehex loads plugin automatically when they are being called.


## default Plugin Folder structure
```
resources/
    |_plugin/
        |_plugin_name/
            |_assets/   (consist of plugin assest file like js, css. e.t.c)
            |_inc/      (consist of plugin class code, class could extends Model1, Controller, Api1 e.t.c)
```

## Common Plugin Function
1. ```current_plugin_asset(...)``` : mostly in use, get current plugin assets path, Current Plugin Assets function Can only be run in Plugin Folder, to retrieve assets of the plugin. E.g Can be use to retrieve plugins/plugin_name/assets/style.css for plugin just list using current_layout_asset("asset_name") *ProjectName->resources->plugins->pluginName*
1. ```resource_path_plugin(...)``` : get the plugin folder path
1. ```plugin_asset(...)``` : get a certain plugin path


## Use Plugin
Plugins are like entire website with full features on it own, therefore, you can call plugin class like you are calling any other app class
and Use the functionality...


## Example
Check out "exDropZone" Plugin in plugin store



