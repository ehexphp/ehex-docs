# Page Dynamic Template

## Introduction
Make all content of a page dynamic and Easy to work by creating a Model for the page.

##
E.g Create HomePage to extends Model1 in models.
Set variable and variable value to be your default value. We Will only make use of ```php HomePage::getDefault( array_of_data = [])->form()->render() ```

i.e
We make use of 
1) ```php HomePage::getDefault( array_of_data = [])->form()->render() ``` : to create and manipulate a form for HomePage
1) ```php HomePage::saveDefault( array_of_data = [])``` : to Save edited data to HomePage
1) ```php HomePage::getDefault()```  : to get saved data as Instance of HomePage
1) ```php HomePage::resetDefault( )```  : clean any data saved on  HomePage


## HomePage Model
```php
    class HomePage extends Model1 {
        // Important Field
        public $header = 'Plan Your Web Project With Ehex';
        public $sub_header = 'The Most Easiest and Powerful Framework';
        public $social_facebook = 'http://...';
        public $social_twitter = 'http://...';
        
        
        // Controller call function, to Save Edited Form data
        static function savePage(){
            if(static::saveDefault($_POST)) Session1::setStatus('Updated', 'Page Updated!', 'success');
            else  Session1::setStatus('Failed', 'Failed to Updated Page', 'error');
        }
    
    
        // Form Renderer function
        static function manage(){
            return self::getDefault()->form([], ['rss_feed'])->setFieldAttribute([
                'our_mission'=>['type'=>'textarea', 'style'=>'height:300px;width:100%', 'class'=>'richeditor']
            ]);
        }
        
        ...

```


## homepage edit page
Create a page for the editing, The code will be simple and light since *HomePage::getDefault( array_of_data = [])->form()->render()* will generate all form field automatically.
We just need to apply few trick to field property. e.g change Some field like "header" from [<input type="text"] to [<textArea.]
```php
 <form action="{{ Form1::callController(HomePage::class, 'savePage()') }}" method="post">
    <div>
        {!! form_token() !!}
        {!! HomePage::manage()->render()  !!}
        <button>  Save </button>
    </div>
 </form>
```



## homepage preview page
General Page to use the dynamic content...
```php
    <?php $homePage = HomePage::getDefault(); ?>
    <li><a href="{{ $homePage->social_twitter }}"><i class="fa fa-twitter"></i></a></li>
    <li><a href="{{ $homePage->social_facebook }}"><i class="fa fa-facebook"></i></a></li>
    <li><a href="{{ $homePage->social_google_plus }}"><i class="fa fa-google-plus"></i></a></li>
    <li><a href="{{ $homePage->social_linked_in }}"><i class="fa fa-linkedin"></i></a></li>
    <li><a href="{{ $homePage->rss_feed }}"><i class="fa fa-rss"></i></a></li>
```



