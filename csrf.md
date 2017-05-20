
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

也可以使用 `Blade` 模板引擎提供的方式：
```php
{!! csrf_field() !!}
```

