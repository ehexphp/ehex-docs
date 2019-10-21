# Ehex Auto DB
Automatic Creation of Database and Model
One of The Beauty of Ehex Is Automatic Creation of Database and Any Model. Just Extend Model1 and call tableCreate(), tableDestroy Method.
E.g, Create a class that Extend Model1, or AuthModel1 for User in app/models
```php
class Book extends Model1 {
    public $id = -1;
    public $title = '';
    public $pages = 0;
    ...

```
Then you can call 
```php
    Book::tableCreate(), Book::tableDestroy() //E.T.C, anywhere
```



## Laravel Blade and Query Builder
To keep it inline with an existing popular System, we make use of similar features, Which are Use of Blade and Query Builder, Which Can be accessible through
Model. Example A Model Of Book.
```php
class Book extends Model1 {
    public $id = -1;
    public $title = '';
    public $pages = 0;
    ...

```
Then you can call 
```php
    Book::query()->where('title', '=', 'bed and med')->get();
```
Which Will return all Available Books in Database with title 'bed and med'.
Meaning All Query Builder are supply through

```php   
    Model1::query()->
```
    
    
    
    
## Simple and Cool Query Build
Execute Database Query With 
```php
Db1::exec( 'SELECT * FROM '.Book::getTableName()  );
// OR
Model1::exec(...);
```
 
Or Just Call 
```php
Model1::help() // on any Model1 Instance class to view other available methods

//You can also use Function like, 
$books = Book::select(...), 
$books = Book::selectMany(...)  
```


    
## Eloquent Loop and Pagination
You can either call paginate separately with array or with $model1->query()->paginate($limit = 10);
```php
     //call  paginate($records, $perPage = 10, $asInfiniteLoad = false, $infiniteLoadConfig = []) ;
    
    //OR manually
    
    //  example paginate( model1->query()  )
    //  example paginate( [1,2,3...10]  )

    //  where Records Could be Any of this
    
    //  $records = [1,2,3,4...100];                             // Php Array
    //  $records = $qb->select('*')->from('sample', 'sample');  // Doctrine
    //  $records = User::select('*')->from('sample');           // Laravel
    //  $records = QB::select('*')->from('sample');             // Pixel // For Ehex

    //  $strana = new \Strana\Paginator();
        $paginator = $strana->perPage(10)->make($records);
        foreach ($paginator as $item) echo $item['field'] . '<br>';
        echo $paginator;
```

Example with Blog List
```php
        <!-- Query -->
        <?php $paginateDataQuery = Blog::query()->select(['title', 'updated_at', 'body', 'id', 'slug', 'category_id', 'user_id'])->orderBy('updated_at', 'DESC')->paginate(3); ?>

        <!-- Loop -->
        @foreach($paginateDataQuery as $row)
            <?php $row = Blog::findOrInit($row) ?>
            <span class="author-name"><a href="#">{{ $row->getAuthor()->name }}</a></span>
            <span class="date"><a href="#">{{ date(DateManager1::$date_asText, strtotime($row->updated_at)) }}</a></span>
            <span class="category"><a href="#">{{ $row->getCategory()->title }}</a></span>
            <p class="post-content"><span class="first-line">{!! String1::getSomeText(Html1::removeTag(String1::getSomeText($row->body, 700, '')), 500) !!}</span></p>
        @endforeach

        <!-- Pagination -->
        {!! $paginateDataQuery !!}
    </div>
```

 ## Want to know more builder ?
 > Visit [pixie](https://github.com/usmanhalalit/pixie)
