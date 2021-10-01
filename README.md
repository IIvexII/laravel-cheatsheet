# Laravel

- [Laravel](#laravel)
    - [Routes](#routes)
    - [Controller](#controller)
    - [View](#view)
    - [Cahce](#cahce)
    - [Blade](#blade)
    - [Migrations](#migrations)
    - [Raw Queries](#raw-queries)
    - [Eloquent](#eloquent)
    - [Eloquent Relationship](#eloquent-relationship)

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
    ```blade
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

### Migrations


* Create Migration file with boilerplate code of posts table
    ```console
        # create table named 'posts'
        php artisan make:migration create_table_post --create='posts'
    ```
* Migrate all the files
    ```console
        php artisan migrate
    ```

* Rollback a migration
    ```console
        php artisan migrate:rollback
    ```

* Make a column in an existing table
    ```console
        php artisan make:migration add_new_col_in_posts --table=posts
    ```
### Raw Queries

* Importing the namespace
    ```php
      use Illuminate\Support\Facades\DB;
    ```
* CRUD
    ```php

    // Creating post
    DB::insert(
        'INSERT into posts (title, content) values (?, ?)',
        ['someTitle', 'Some Content']
    );    
    
    // Reading
    DB::select('SELECT * FROM posts');

    // Updating
    DB::update(
        'UPDATE posts SET title = "Demo" where title = ?',
         ['someTitle']
    );

    // Deleting
    DB::delete('DELETE FROM posts WHERE title = ?', ['Demo']);
    ```

### Eloquent

  * Create a model
    ```console
    # -m for migration
    php artisan make:model Post -m
    ```

  * Rading from database
    ```php
        $posts = Post::all();
    ```

  * Search in the database
    ```php
        $id = 1;
        // find the posts from table where id = 1
        $post = Post::find($id);

        // via where('column name', 'matching value')
        $post = Post::where('id',$id);

        // print the title of that post
        echo $post->title;
    ```

    * Creating record
        ```php
            // create new instence
            $posts = new Post;

            // Setting the properties.
            // These are the same as the columns of that table 
            $posts->title = "Some Title";
            $posts->content = "Some Content";

            // Saving data into the database.
            $posts->save();

            // 2nd method is below
            Post::create([
                'title' => 'Some Title',
                'content' => 'Some Content'
            ]);
        ```
    * Delete Record
        ```php
            // 1st method
            $post = Post::find($id);
            $post->delete();

            // 2nd method
            Post::destroy($id);
        ```

### Eloquent Relationship

* hasOne() relation
  * Suppose we have a relation of credit card with customer which is one to one which means every customer will have 1 credit card.
  
  App\Models\Customer.php
  ```php
    <?php

    namespace App\Models;

    use Illuminate\Database\Eloquent\Factories\HasFactory;
    use Illuminate\Database\Eloquent\Model;

    class Customer extends Model
    {
        public function creditCard()
        {
            // One to One relationship
            /*
            \--------------------------------------------
            \ It means each customer hasOne credit card.
            \--------------------------------------------
            */
            return $this->hasOne('App\Models\CreditCard');

            // Parameters:
            /*
              1. Model name with which we are creating relation
              2. (optional) name of column in relational table with which we are relating to like customer_id.
              3. (optional) name of column our customer table like id.
            */
        }
    }
  ```
  * We need one credit card model and should have one customer_id or
any unique column which represent to whome it blongs to.
  * We can now get card belongs to a customer via this:
    ```php
        Customer::find($id)->creditCard;
    ```