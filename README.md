# Laravel

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