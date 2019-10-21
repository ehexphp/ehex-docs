# Model View Controller (MVC)
The main reason for MVC is to get a grip on the logic to drive your application. MVC allows you to clearly separate things that should be separate: The model, code which converts the model value for the display and the code which controls the model. So this is not related to HTML or URLs in any way.
Therefore, we create folder structure for it. under your "Project Name/"

```
app
 |_models
 |_controllers
 |_api
```
 
Each model class must extends Model1 abstract class, each controller class must extends Controller1 abstract class, 
And each api class must extends Api1 abstract class.

Note: For Internal Usage (i.e, usage within the application), Any Model1 extended class is powerful enough to serve as either Model, Controller or Api function holder.
I.e All your functions can stay in Model Class if you are feeling lazy or wish to hold all in one File.


Api class should be called with **Form1::callApi("Book::addBook()")**,  While
Controller class should be called with **Form1::callController("Book::addBook()")**
Please check API and Controller for detail info...















