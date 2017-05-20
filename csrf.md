## CSRF 攻击

> 跨站请求伪造是 `一种通过伪装授权用户的请求来利用授信网站的漏洞。`

Laravel 自动为每一个被应用管理的有效用户会话生成一个 `CSRF` 令牌，该令牌用于验证授权用户的发起请求者是否是同一个人。

可以利用 `csrf_field()` 帮助函数来生成一个包含 `CSRF` 令牌的隐藏输入字段。

```php
<?php echo csrf_field(); ?>
```
辅助函数 csrf_field 会生成如下 HTML：
```html
<input type="hidden" name="_token" value="<?php echo csrf_token(); ?>">  
```

```html
<input type="hidden" name="_token" value="{{ csrf_token() }}">  
```

也可以使用 `Blade` 模板引擎提供的方式：
```php
{!! csrf_field() !!}
```

### 从 `CSRF` 保护中排除指定 `URL`

> 可以在 `VerifyCsrfToken` 中间件中将需要排除的 `URL` 添加到 `$except` 属性中。

```php
<?php

namespace App\Http\Middleware;

use Illuminate\Foundation\Http\Middleware\VerifyCsrfToken as BaseVerifier;

class VerifyCsrfToken extends BaseVerifier
{
    /**
     *从CSRF验证中排除的URL
     *
     * @var array
     */
    protected $except = [
        'stripe/*',
    ];
}
```
### `X-CSRF-Token`

除了将 `CSRF` 令牌作为 `POST` 参数进行验证外，还可以通过设置 `X-CSRF-Token` 请求头来实现验证，`VerifyCsrfToken` 中间件会检查 `X-CSRF-TOKEN` 请求头，首先创建一个 `meta` 标签并将令牌保存到该 meta 标签：

```html
<meta name="csrf-token" content="{{ csrf_token }}">  
```

然后在 `js` 库（如 `jQuery`）中添加该令牌到所有请求头，这为基于 `AJAX` 的应用提供了简单、方便的方式来避免 CSRF 攻击：

```
$.ajaxSetup({
    headers: {
        'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content')
    }
});
```

### `X-XSRF-Token`

`Laravel` 还会将 `CSRF` 令牌保存到了名为 `XSRF-TOKEN` 的 `Cookie` 中，你可以使用该 `Cookie` 值来设置 `X-XSRF-TOKEN` 请求头。一些 `JavaScript` 框架，比如  `Angular`，会为你自动进行设置，基本上你不太需要手动设置这个值。