# Laravel
[<TIL](Programming.md)

## Eloquent return only/latest n rows

* get all rows order by `created_at` **if $timestamps=true**
```
$incidents = Incident::where('monitor_id','=',1)->latest()->get();
```
* order by a specific column get last 3
```
$incidents = $monitor->incidents()->orderBy('started_at','desc')->take(3)->get();
```
* get the last 10 rows order by primary key
```
$incidents = Incident::where('monitor_id','=',1)->take(10)->get();
```

## Redirect to Controller method with parameters

```php
return redirect()->action('SomeController@method', ['param' => $value]);
```

## dd() at the end of a Eloquent sentence
```php
$users = User::where('name', 'Mary')->get()->dd();
```
[source](https://twitter.com/dailylaravel/status/1046716110463291392)

## auth() in Blade
To check whether an use is authenticated or not, use the `@auth` directive:
```html
@auth
 <h1> You are now authenticated! </h1>
@endauth
```

## hasMany with specific checks
Filter out records that have _n_ amount of children records:
```php
$authors = Author::has('books', '>', 5)->get();
```

## Where date methods in Eloquent
```php
$items = Item::whereMonth('created_at', '12')->get();
$items = Item::whereDay('created_at', '31')->get();
$items = Item::whereYear('created_at', '2017')->get();
$items = Item::whereDate('created_at', '2017-01-31')->get();
$items = Item::whereTime('created_at', '12:34:56')->get();
```
[source](https://laravel-news.com/eloquent-tips-tricks)
## Run specific migration directory
`php artisan migrate --path="app/some_folder/some_migrations_folder"`
