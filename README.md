# Laravel

1. [Routes](#Routes)
2. [Controller](#Controller)

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

* This will create a basic controller
```console
PS C:\project> php artisan make:controller postController
```

* Create a controller with resource methods like index, create, show, etc
```console
PS C:\project> php artisan make:controller --resource postController
```
### View

```php

view('filename placed in view folder');

// associative array is optional
view('filename placed in view folder', [
    'variable_name' => "value"
    ]);
```

### Cahce

Cache will allow to store data temporarily during the time mentioned.

```php
cache()->remember('variable_name', timeInSeconds, function () => use (external_variables){
    // Get the file content
    return file_get_contents('file_path');
});
```