Избавляемся от index.php при использовании nginx
================================================

Необходимо немного поменять конфиг nginx.

Добавляем PATH_INFO:
```
location ~ \.php {
    fastcgi_pass  127.0.0.1:9000;
    fastcgi_index index.php;
    include fastcgi_params;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_param PATH_INFO $fastcgi_script_name;
}
```

Переписываем пути (для nginx >= 0.6.36):
```
location /yiiGuestbook {
    try_files $uri $uri/ /yiiGuestbook/index.php?$query_string;
}
```


Переписываем пути (для nginx < 0.6.36):
```
location /yiiGuestbook {
    if (!-e $request_filename){
        rewrite (.*) /yiiGuestbook/index.php/$1;
    }
}
```

Вот это должно быть в `fastcgi_params`

```
fastcgi_param  QUERY_STRING       $query_string;
fastcgi_param  REQUEST_METHOD     $request_method;
fastcgi_param  CONTENT_TYPE       $content_type;
fastcgi_param  CONTENT_LENGTH     $content_length;

fastcgi_param  SCRIPT_NAME        $fastcgi_script_name;
fastcgi_param  REQUEST_URI        $request_uri;
fastcgi_param  DOCUMENT_URI       $document_uri;
fastcgi_param  DOCUMENT_ROOT      $document_root;
fastcgi_param  SERVER_PROTOCOL    $server_protocol;

fastcgi_param  GATEWAY_INTERFACE  CGI/1.1;
fastcgi_param  SERVER_SOFTWARE    nginx/$nginx_version;

fastcgi_param  REMOTE_ADDR        $remote_addr;
fastcgi_param  REMOTE_PORT        $remote_port;
fastcgi_param  SERVER_ADDR        $server_addr;
fastcgi_param  SERVER_PORT        $server_port;
fastcgi_param  SERVER_NAME        $server_name;

# PHP only, required if PHP was built with --enable-force-cgi-redirect
fastcgi_param  REDIRECT_STATUS    200;
```

Если вы не выносите приложение за корень, не лишним будет закрыть доступ к
некоторым директориям:

```
location ~ /(protected|themes/classic/views)/ {
    deny all;
}
```

Остальное делается также, как и в случае с Apache: [Красивые адреса URL](/doc/guide/ru/topics.url)

---
  - `Основано на`: [http://www.yiiframework.com/doc/cookbook/15/](http://www.yiiframework.com/doc/cookbook/15/)
  - `Перевод`: Александр Макаров, Sam Dark ([rmcreative.ru](http://rmcreative.ru/))
  - `Дополнения`: [Stamm](http://yiiframework.ru/forum/memberlist.php?mode=viewprofile&u=783)
  - `Обсуждение и комментарии`: [http://yiiframework.ru/forum/viewtopic.php?f=8&t=12](http://yiiframework.ru/forum/viewtopic.php?f=8&t=12)
