# software config

记录并保存了常用软件的配置规则



* **wamp server(Apache)**

  > 配置有效的缓存策略

  > 配置文件 httpd.conf

  ```bash
  # 第一种方法
  LoadModule expires_module modules/mod_expires.so
  ExpiresActive On
  # html文档的过期时间为 从上次访问（A）开始 1000 秒钟
  ExpiresByType text/html A1000
  ExpiresByType image/gif A2592000
  # HTML文档的有效期是最后修改（M）时刻后的一星期
  ExpiresByType text/html M604800
  ExpiresByType text/css N1000
  ExpiresByType text/js "now plus 2 days"
  ExpiresByType image/jpeg "access plus 2 months"
  ExpiresByType image/bmp "access plus 2 months"
  ExpiresByType image/x-icon "access plus 2 months"
  ExpiresByType image/png "access plus 2 months"
  
  # 第二种方法
  LoadModule headers_module modules/mod_headers.so
  header set cache-control "max-age=1000"
  ```

  