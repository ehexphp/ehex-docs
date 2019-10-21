

# Introduction
To Work Perfectly in Ehex, We have prepare a perfect work flow. Place in intending template in 
**projectName->resources->views->layouts**
##
Reason for putting your template folder in Layouts's folder is because, it assets can easily be located with ```php "layout_asset()" ``` function.
More so, it maintains project folder structure creating uniformity and neatness for your codes, makes other developer to understand the general arrangement as well.

## Example for index.blade.php
```php
    <?php $homePage = HomePage::getDefault(); ?>
    @extends('layouts.turbo.template', ['page_title'=>'Xamtax Technology - HomePage'])
    
        
    @section('page_content')
        <div id="main-wrapper">
    
    
            <div class="container">
                <h1>{!! $homePage->header !!}</h1>
            </div>
            ...
        </div>
    @endsection
                    
 ```
 
 
 
 
 
 
 
 
 
 ## Where the extendend template of "layouts.turbo.template" is located in layout/turbo/ as template.blade.php
 template.blade.php
 ```php
     <!DOCTYPE html>
     <html lang="en">
       <head>
         <meta charset="UTF-8">
         <meta http-equiv="X-UA-Compatible" content="IE=edge">
         <meta name="viewport" content="width=device-width, initial-scale=1">
         <title>{{ $page_title }}</title>
         
          <!-- Default Plugins -->
          <link href="{{ shared_asset() }}/fonts/css/font-awesome.min.css" rel="stylesheet">
          <link href="{{ shared_asset() }}/sweetalert2/sweetalert2.min.css" rel="stylesheet"/>
          <script src="{{ shared_asset() }}/sweetalert2/sweetalert2.min.js"></script>
    
         <link href="{{ layout_asset() }}/js/style.css" rel="stylesheet"/>
         <script type="text/javascript" src="{{ layout_asset() }}/js/scripts.js"></script>
         </head>
         <body>
         
         
         ....
         
         
       </body>
     </html>
 
     <!-- Print Notification -->
     <?php Session1::popupStatus()->toSwalAlert();  ?> 
  ```
 