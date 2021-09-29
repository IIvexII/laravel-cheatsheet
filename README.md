# Laravel

- [Laravel](#laravel)
    - [Routes](#routes)
    - [Controller](#controller)
    - [View](#view)
    - [Cahce](#cahce)
    - [Blade](#blade)

### Routes

```php
// We can use rout to make urls for our website
// Simple method
Route::get('/home', fn()=>view('homePage'));

// Sending the home page a set of values via
// associated array. 'var' will become variable
// in the home page.
Route::get('/home', function (){
    return view('homePage', [
                'var'=> 'value'
            ]);
});
// Named Route
Route::get('/admin/home', function(){
    // this will print the full url by just
    // using the shortName or nick name.
    $url = route('shortName');

})->name('shortName');
```

### Controller

* Create Controller
    * This will create a basic controller
    ```console
    PS C:\project> php artisan make:controller homeController
    ```

    * Create a controller with resource methods like index, create, show, etc
    ```console
    PS C:\project> php artisan make:controller --resource homeController
    ```
* Use it in Routes
  * Basic Routing
    ```php
        // including the controller
        use App\Http\Controllers\homeController
        // Routing to the controller named homeController
        // and running the method name index()
        Route::get('/', [homeController::class, 'index']);
    ```

  * Passing Parameter
  ```php
  // parameter will be passed to the function index($name)
  // It will throw error if the index() does not take para-
  // meter
  Route::get('/post/{name}', [postController::class, 'index']);
  ```

  * Using resource
  ```php
  // resource will automatically create the urls according
  // to the methods of the controller
  Route::resource('post', postController::class);
  ```

### View

```php
// Filename without blade.php extension
view('filename');

// Passing data to view
view('filename placed in view folder', [
    'variable_name' => "value"
    ]);

view("filename")->with('var_name', 'value');

view('filename', compact('var_name'));
```

### Cahce

Cache will allow to store data temporarily during the time mentioned.

```php
cache()->remember('variable_name', timeInSeconds, function () => use (external_variables){
    // Get the file content
    return file_get_contents('file_path');
});
```

### Blade

* Layouts
  * app.blade.php
    ```php
        {{$var_name}}   // it will print value of variable

        /* 
            Extands the page with basic layout
            and reusable code like header and
            footer.
        */
        @extends('layouts.templateName')

        // Used in template layout which place the
        // section with same section name to its place.
        @yield('sectionName')

        @section('sectionName')
            // section code inside in html/css/js
        @endsection

        @if (condition)
            <!-- code -->
        @endif

        @foreach($arr as value)
            <!-- code -->
        @endforeach
    ```
