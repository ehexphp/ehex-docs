# Widget

## Introduction
Widget are more control or component added together for a purpose.
Lot of Widget are built around ehex to make developer deliver on time. 


## exWidget1
> This comprise of popular widget like live chat, google analytics, disqus comment e.t.c

### Disqus Comment
We added Disqus Comment because it is popular and fast [Disqus Website](https://disqus.com/admin/create/) to Create an account and copy the app name or sub-domain of the given link.
```php
    // insert app_id in config widget disqus key
    static function widget($key){
        return [
            'disqus'=>'ehex',
            ...
    
    
    // put widget at the end of your template or page
    echo exWidget1::getDisqusCommentBox()
```




### Live Chat with JIVO
We added Jivo livechat because it has some cool features. To use Jivo LiveChat in Ex, Please Visit [Jivo Website](https://www.jivochat.ng?partner_id=21855&pricelist_id=25&lang=en) to Create an account and copy the widget_id.
```php
    // insert widget_id in config widget jivo_livechat key
    static function widget($key){
        return [
            'jivo_livechat'=>'RR6Sdm1ShQ',
            ...
    
    
    // put widget at the end of your template or page
    echo exWidget1::getJivoLiveChat()
```




### Tawk Live Chat
[Website](https://dashboard.tawk.to/) to Create an account and copy your unique id
```php
    // insert id in config widget
    static function widget($key){
        return [
            'tawk_livechat'=>'5c40202cab5284048d0d52cd',
            ...
    
    
    // put widget at the end of your template or page
    echo exWidget1::getTawkLiveChat()
```



### Google Analytics Chat
[Website](https://analytics.google.com) to Create an account and copy your unique id
```php
    // insert id in config widget
    static function widget($key){
        return [
            'google_analytics'=>'UA-131446983-1',
            ...
    
    
    // put widget at the end of your template or page
    echo exWidget1::getGoogleAnalytics()
```





## Html1Widget1
> This comprise of popular widget like toast, menuList, panel, ImageUpload, ImageDelete e.t.c
