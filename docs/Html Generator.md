# Html Generator

## Smart HtmlForm Generator Library
**Quick Html Form Control** (Default is Bootstrap, But it's Flexible and easy to Tweak to other UI. Just assign ui class name to  HtmlForm1::$THEME_ blabla at the beginning of any template page or Config1::onPageStart() for general effect on all page)
- HtmlForm1 Consist of static method for generating a quick form control

**HtmlForm1** : Consist of Html Form helper to generate a fast bootstrap control e.g
HtmlForm1::addInput('label name', ['attribute1'=>'value1', 'attribute2'=>'value2', ... ])


***
For Example
```php
    echo HtmlForm1::addInput('User Name', $inputAttribute = ['name'=>'user_name', 'value'=>'Samson Iyanu' ]);
```
Would Generate

```php
    <div class="form-group"> 
      <label class="control-label" for="user_name">
          User Name
      </label> 
      <input class="form-control  input-lg" type="text" name="user_name" placeholder="User Name" />
    </div>
```


Other Examples are 

```php
- HtmlForm1::addHidden('user_id', '1');

- HtmlForm1::addInput("Full Name", ['name'=>'full_name', 'value'=>'Samson Iyanu' ]);

- HtmlForm1::addTextArea("Description", ['name'=>'description', 'value'=>'i am a developer' ]);

- HtmlForm1::addSelect("Sex", ['name'=>'sex', 'value'=>['male', 'female'] ]);

e.t.c
```


## Attribute Builder
This attribute is very the same with normal html input attribute, we only added some additional attribute for additional features like toggle attribute for password and we normalize all input content attribute as value. therefore even for textarea and select tag, they all accept value attribute.

```echo HtmlForm1::addTextArea('About Me', ['value'=>'hello world', 'required'])```


## Attribute Append
Attibute could be appended with either default attribute or custom define, simply by prepending plus sign. (+).

```echo HtmlForm1::addInput('User Name', ['+style'=>'background:red', '+class'=>'form-control-lg', 'value'=>'hello world', 'required'])```



    



## Wrap Method
* HtmlForm1::open('methodName', ['formAttribute1'=>'value1']) : this generate <form action=''> tag and accept controller name directly. it has an inbuilt Form1::callController(...). 
* HtmlForm1::close('buttonValue', ['buttonAttribute1'=>'value1']) : this generate </form> tag, it then generate submit button automatically along if button value name is passed in. default is null

##  Methods

* HtmlForm1::open(...) :            - generate ```<form action='' >```
* HtmlForm1::close(...) :           - generate ```</form >``` with submit button

* HtmlForm1::addInput(...)          - generate full form control wrap around ```<input type='text' ...>```
* HtmlForm1::addTextArea(...)       - generate full form control wrap around ```<textarea ... > ... </textarea>```
* HtmlForm1::addSelect(...)         - generate ```<select >...</select>```

* HtmlForm1::addFile(...)   :       - or you can use HtmlWidget1::imageUploadBox(...) for direct upload and image preview
* HtmlForm1::addHidden(...) :       - generate ```<input type='hidden'>```

* HtmlForm1::addPanel(...)
* HtmlForm1::addModal(...)
* E.T.C




Example
```php
    {{ HtmlForm1::addInput('User Name', ['name'=>"user_name", 'value'=>'Samson Iyanu', 'placeholder'=>'input name', 'required']) }}
````
Would Generate Something like this in Bootstrap
```php
    <div class="form-group"> <label class="control-label" for="user_name">User Name</label> <input class="form-control input-lg" type="text" name="user_name" value="Samson Iyanu" placeholder="input name" required /></div>
````

Others Are 
```php
    {!! HtmlForm1::addInput('User Name', ['name'=>"user_name"]) !!}
    
    {!! HtmlForm1::addInput('Phone Number', ['name'=>"phone_numbebr", "type"=>"number"]) !!}
    
    {!! HtmlForm1::addInput('Password', ['name'=>"password", 'type'=>'password', 'required' ]) !!}
    
    {!! HtmlForm1::addSelect('Sex', ['name'=>"sex", 'value'=>['male'=>'Male', 'female'=>'Female'], 'selected'=>'female']) !!}
    
    {!! HtmlForm1::addSelect('Country', ['name'=>"country", 'value'=>['usa'=>'United State', 'nigeria'=>'Nigeria'], 'selected'=>'nigeria', 'link'=>'http://data.com/country.json']) !!} // Select DropDown with Ajax, **link** attribute required. This must fetch json of array
    
    {!! HtmlForm1::addTextArea('Address', ['name'=>"address", 'value'=>'']) !!}
```


Even any Model1 Inherited Class Could Call $user->form() or User::formMake() which could use for data creation or data edit.

Example
```php
    User::formMake(null, [], ['role', 'avatar', 'created_at', 'updated_at', 'id', 'last_login_at'])
    ->setTitle('User List')
    ->setSimilarFieldAttribute(['full_name', 'password', 'user_name'], ['required'])
    ->setFieldAttribute([
        'sex'=>['value'=>['male'=>'Male', 'female'=>'Female']],
        'country'=>['type'=>'select',  'value'=>Array1::reUseValueAsKey(EasyCountry::getCountries())],
        'level'=>['type'=>'select',  'value'=>Array1::reUseValueAsKey(User::getLevel()), ],
     ])
    ->render()
```






### HtmlWidget for Quick Html Widget Call...
* HtmlWidget1::imageUploadBox(...)
* HtmlWidget1::listAndMarkActiveLink(...)
* HtmlWidget1::menuHorizontalBar(...)
* HtmlWidget1::rating(...)
* HtmlWidget1::menuHorizontalBarAdmin(...)
* HtmlWidget1::toast(...)
* HtmlWidget1::box1(...)
* HtmlWidget1::box2(...)
* HtmlWidget1::listData(...)
* HtmlWidget1::listDataKeyValue(...)
* HtmlWidget1::listLink(...)
* HtmlWidget1::listCheckBox(...)
* HtmlWidget1::listRadioButton(...)
* HtmlWidget1::textHeader(...)
* HtmlWidget1::flipperWidget(...)
* HtmlWidget1::articlePage(...)
* E.T.C




## HtmlStyle1 for Quick Html Style Call...
* HtmlStyle1::enablePreWrap(...)
* HtmlStyle1::enableCenter(...)
* HtmlStyle1::getFixBackgroundAttr(...)
* HtmlStyle1::getShadow2x(...)
* HtmlStyle1::HtmlAsset1(...)
* E.T.C


## HtmlAsset1
* HtmlAsset1::getImageAvatar(...) - e.g 
    ```php
    <img src='<?php echo HtmlAsset1::getImageAvatar() ?>' />
    
    <img src='<?php echo HtmlAsset1::getImageThumb() ?>' />
  
    ```
* HtmlAsset1::getImageThumb(...)
* E.T.C


## Convert Array to Table
To Convert Array to table use Array1::toHtmlTable()
```php
    echo Array1::toHtmlTable(User::all(), ['user_name', 'address', 'action'], [], [], function($key, $row){
        if($key == "action")
            return "<a class='btn btn-danger' href='".url("/$row[id]/delete")."'>Delete</a> ";
        else
            return $row[$key];
    })
```