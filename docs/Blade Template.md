
# Introduction
Blade is the simple, yet powerful templating engine provided with Ehex. Unlike other popular PHP templating engines, Blade does not restrict you from using plain PHP code in your views. In fact, all Blade views are compiled into plain PHP code and cached until they are modified, meaning Blade adds essentially zero overhead to your application. Blade view files use the  .blade.php file extension and are typically stored in the resources/views directory.

## Common View Function
1. ```view(...)``` OR exBlade1::renderView() : Compile View and render it on the screen.
1. ```view_make(...)``` OR exBlade1::getCompiledView() : Compile View and return phtml version.
1. ```view_exists(...)``` OR exBlade1::viewExists() : is view available in any view directory (both project view and shared view).
1. ```viewpath_to_path(...)``` OR exBlade1::convertViewPathToPath() : Get View full path
1. ```path_to_viewpath(...)``` OR exBlade1::convertPathToViewPath() : Get ViewPath Relative Path From FullPath
1. ```get_all_view_in_directory(...)```: get all view in directory

1. ```register_path_for_layout_asset()```: Put at the beginning of the layout template. This will set ```layout_asset(...)``` to layout path
1. ```layout_asset(...)``` : get the current layout theme in use, when you extend layout in your view, works with ```register_path_for_layout_asset()``` or specify your layout_name manually file path url in *ProjectName->resources->view->layouts->themeName*
1. ```current_layout_asset(...)``` : get current file layout assets, Current Layout Assets function Can only be run in Layout Folder, to retrieve assets of the layout. E.g Can be use to retrieve layouts/plugin_name/assets/style.css for plugin instead of using layout_asset("layout-name") *ProjectName->resources->view->layouts->themeName*



## Defining A Layout
Two of the primary benefits of using Blade are template inheritance and sections. To get started, let's take a look at a simple example. First, we will examine a "master" page layout. Since most web applications maintain the same general layout across various pages, it's convenient to define this layout as a single Blade view:

<!-- Stored in resources/views/layouts/app.blade.php -->
```php
<html>
    <head>
        <title>App Name - @yield('title')</title>
    </head>
    <body>
        @section('sidebar')
            This is the master sidebar.
        @show

        <div class="container">
            @yield('content')
        </div>
    </body>
</html>

```



As you can see, this file contains typical HTML mark-up. However, take note of the @section and @yield directives. The @section directive, as the name implies, defines a section of content, while the @yield directive is used to display the contents of a given section.

Now that we have defined a layout for our application, let's define a child page that inherits the layout.


## Extending A Layout
When defining a child view, use the Blade @extends directive to specify which layout the child view should "inherit". Views which extend a Blade layout may inject content into the layout's sections using @section directives. Remember, as seen in the example above, the contents of these sections will be displayed in the layout using @yield:

<!-- Stored in resources/views/child.blade.php -->
```php
@extends('layouts.app_template_theme')

@section('title', 'Page Title')

@section('sidebar')
    @parent
    <p>This is appended to the master sidebar.</p>
@endsection

@section('content')
    <p>This is my body content.</p>
@endsection

```
In this example, the sidebar section is utilizing the @parent directive to append (rather than overwriting) content to the layout's sidebar. The @parent directive will be replaced by the content of the layout when the view is rendered.



## Extending a Layout As Theme
To use a particular layout as a theme,  For Example
```php
views
  |_layouts
      |_w3ShoppingTemplate  : Theme Name
         |_assets   : This consist of theme resources like css, js and icons.
            |_css
            |_js 
         |_inc      : This consists of auto loadable class for your theme, this can be Models, Page Data Class e.t.c
         |_template.blade.php
         
  |_pages
      |_index.blade.php : This Can extended "w3ShoppingTemplate" and all the assets data will be usable through *layout_asset()*
```

On top of *template.blade.php* file content, insert ```register_path_for_layout_asset()``` OR ```exUrl1::registerPathForLayoutAsset()```, This will make use of the template path to locate the assets path resources. This can also be located automatically if the template truly stays in layout directory
To Use Template Assets Path. Use ```layout_asset()``` this will locate layout assets resources.
E.g

```php
    <link rel="stylesheet" href="{{ layout_asset() }}/css/dashboard.css">
    <script type="text/javascript" src="{{ layout_asset() }}/js/scripts.js"></script>
```
Makes it portable and possible to keep and share layout template theme with friends and teams

> You can  read this article on blade layout template [simple-laravel-layouts-using-blade](https://scotch.io/tutorials/simple-laravel-layouts-using-blade)

## If Statements
You may construct if statements using the @if, @elseif, @else, and @endif directives. These directives function identically to their PHP counterparts:
```php
@if (count($records) === 1)
    I have one record!
@elseif (count($records) > 1)
    I have multiple records!
@else
    I don't have any records!
@endif
```
For convenience, Blade also provides an @unless directive:




## Loops
In addition to conditional statements, Blade provides simple directives for working with PHP's loop structures. Again, each of these directives functions identically to their PHP counterparts:
```php
@for ($i = 0; $i < 10; $i++)
    The current value is {{ $i }}
@endfor

@foreach ($users as $user)
    <p>This is user {{ $user->id }}</p>
@endforeach

@forelse ($users as $user)
    <li>{{ $user->name }}</li>
@empty
    <p>No users</p>
@endforelse

@while (true)
    <p>I'm looping forever.</p>
@endwhile
```




## When using loops you may also end the loop or skip the current iteration:
```php
@foreach ($users as $user)
    @if ($user->type == 1)
        @continue
    @endif

    <li>{{ $user->name }}</li>

    @if ($user->number == 5)
        @break
    @endif
@endforeach
You may also include the condition with the directive declaration in one line:

@foreach ($users as $user)
    @continue($user->type == 1)

    <li>{{ $user->name }}</li>

    @break($user->number == 5)
@endforeach
```





## The $loop variable also contains a variety of other useful properties:

Property	Description
$loop->index	The index of the current loop iteration (starts at 0).
$loop->iteration	The current loop iteration (starts at 1).
$loop->remaining	The iterations remaining in the loop.
$loop->count	The total number of items in the array being iterated.
$loop->first	Whether this is the first iteration through the loop.
$loop->last	Whether this is the last iteration through the loop.
$loop->depth	The nesting level of the current loop.
$loop->parent	When in a nested loop, the parent's loop variable.

Comments
Blade also allows you to define comments in your views. However, unlike HTML comments, Blade comments are not included in the HTML returned by your application:
```php
{{-- This comment will not be present in the rendered HTML --}}

PHP
In some situations, it's useful to embed PHP code into your views. You can use the Blade @php directive to execute a block of plain PHP within your template:

@php
    //
@endphp
```





Even though the included view will inherit all data available in the parent view, you may also pass an array of extra data to the included view:

## Include
```php
@include('view.name', ['some' => 'data'])
Of course, if you attempt to @include a view which does not exist, Ehex will throw an error. If you would like to include a view that may or may not be present, you should use the  @includeIf directive:

@includeIf('view.name', ['some' => 'data'])
```


> Check Laravel Blade Documentation for more...  [Blade Documentation Class](https://laravel.com/docs/5.6/blade)

