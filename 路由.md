## 路由

### 基础路由

```php
Router::get('basic', function() {
    return "Hello World!";
});
```

### 多请求路由

```php
Route::match(['get', 'post'], 'multy', function(){
    return "multy";
});
```

```php
Route::any('multy2', function(){
    return "multy2";
});
```

### 路由参数

```php
Route::get('user/{id}', function($id){
    return 'User-id-' . $id;
});
```

```php
Route::get('user/{name?}', function($name = null){
    return 'User-name-' . $name;
});
```
> 上面 `name` 后面跟 `?` 表示可以有也可以没有。

```php
Router::get('user/{name?}', function($name = null){
    return 'User-name-' . $name;
})->where('name', '[A-Zz-z]+');
```
> 这里后面的 `where` 用来对 `name` 进行验证。

### 路由别名

```php
Router::get('user/center', ['as' => 'center', function(){
    return route('center');
}]);
``` 
> 这里可以用 `as` 来给路由起一个别名。

```php
Router::get('user/center', function(){
    return route('center');
})->name('center');
```
> 也可以在路由定义以后使用 `name` 方法来定义一个别名。

### 路由群组

```php
Route::group(['prefix' => 'member'], function(){
    Route::any('multy2', function() {
        return 'member-multy2';
    });
});
```
> 这里就是给里面的路由添加一个前缀 `member` ,即里面的路由实际是 `member/multy2` 。

### 路由中输出视图

```php
Route::get('view', function(){
    return view('welcome');
});
```
> 访问 `view` 路由的时候, 输出视图 `welcome` 。

### 路由与控制器关联

```php
Router::get('member/info', 'MemberController@info');
```

```php
Route::any('member/info', ['uses' => 'MemberController@info']);
```
> 以上两种方式都可以使用控制器对应的方法。

### 中间件在路由中使用

```php
Route::group(['middleware' => 'web'], function(){
    Route::any('/', ['uses' => 'StudentController@login']);
});
```
> 这里使用了 `web` 中间件。

