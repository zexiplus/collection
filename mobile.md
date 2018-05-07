### mobile-development-caveats

1. **meta**

```html
<!DOCTYPE html>
<html>
  <head>
    <meta 
       name="viewport" content="user-scalable=no, width=device-width, 
       initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, minimal-ui"/>
    <meta name="screen-orientation" content="portrait"/>
    <meta name="format-detection" content="telephone=no" />
  </head>
  <body>
  </body>
</html>
```

 viewport `minimal-ui`  最小化ui及使浏览器导航栏默认不显示

screen-orientation `portrait` 强制竖屏显示

format-detection `telephone=no` 手机号码不被显示为拨号连接



