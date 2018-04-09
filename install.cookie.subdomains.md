Cookie на поддоменах
=========================

Если сайт использует поддомены, то целесообразно авторизовать пользователя одновременно как на основном домене, так и на поддоменах.

Пример: основной домен сайта http://example.com. У него есть поддомены http://msk.example.com, http://spb.example.com.

Пользователь авторизировался на http://example.com, но на http://msk.example.com он уже будет "гостем", а не "авторизированным пользователем".

Чтобы избежать такого досадного недоразумения необходимо добавить в конфигурационный файл (например: `protected\config\main.php`), в секцию `components`, следующий код:

```php
'session' => array(
    'cookieParams' => array('domain' => '.domain_site.ru'),
),
```

и

```php
'user' => array(
    // enable cookie-based authentication
    'allowAutoLogin' => true,
    'identityCookie' => array('domain' => '.domain_site.ru'),
),
```

В итоге должно выглядеть так:

```php
...
// application components
'components' => array(
    ...
    'session' => array(
        'cookieParams' => array('domain' => '.domain_site.ru'),
    ),
    ...
    'user' => array(
        // enable cookie-based authentication
        'allowAutoLogin' => true,
        'identityCookie' => array('domain' => '.domain_site.ru'),
    ),
    ...
),
...
```

> Note|Примечание: После внесённых изменений необходимо очистить cookie в браузере и очистить кэш сайта.

Обычно, кэш самого скрипта хранится в `protected/runtime/cache`.

Однако, можно задать свою папку для хранения кэша. Для этого необходимо после строки:

```php
'basePath' => dirname(__FILE__).DIRECTORY_SEPARATOR.'..',
```

Добавить код:

```php
'runtimePath' => Yii::getPathOfAlias('system').'/../my_cache_folder/',
```

После изменений кэш будет храниться от "корня" сайта (где находится index.php) в папке "my_cache_folder".

---
 - `Автор`: Сергей Жолобов, Xpycm ([monoray.ru](http://monoray.ru/)).