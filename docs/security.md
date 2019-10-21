# Security
> Security functions are  ```token()``` and ```encrypt_data()```

## Token
token is required in form and api request. i.e every form request must include token field somehow.
either through form_token() (which generate html form field) or manually 
```php
    <input type="hidden" value="{{ token() }}" name="token" /> 
```
          
## encrypt_data and encrypt_validate()
encrypt_data() function use Config1::APP_KEY cryptographic salt to encrypt any string passed in into one way hashed string. This is used to encrypt user password as well as other sensitive login data.
```php
    // hashed data
    $password_hash = encrypt_data($_REQUEST['password']);
    
    // verify if database hashed password thesame as user input password
    $isPasswordValid = encrypt_verify($_REQUEST['password'], $password_hash);
```



## encode and decode data
Since ```encrypt_data()``` function is a one-way encryption, we added two-way encoder function as well.
Which are  ```encode_data($data = '', $password = Config1::APP_KEY)``` and ```decode_data($cipheredData = '', $password = Config1::APP_KEY)``` which uses your ```Config1::APP_KEY```   by default to encode and decode. 

```php 
    $text = 'Ehex EX with full easy features...';
    $encode_text = encode_data($text);
    $decode_text = decode_data($encode_text);

    dd( $decode_text );
```


## CLF
please read about [Api and Controller CLF Restriction](Api%20and%20Controller%20Call.md), With this, you can regulate the method you give access to through CLF 
