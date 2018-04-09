Использование приложения Yii в сторонних скриптах
=================================================

Иногда возникает потребность работать с данными из приложения Yii в стороннем скрипте,
например, вывести количество записей в таблице или произвести иные действия.
Удобней всего делать это, используя классы Yii.

Чтобы задействовать все возможности Yii в стороннем коде, скопируйте
файл `index.php` в, например, `yiiapp.php`, убрав в нём непосредственно
запуск приложения:
```php
// change the following paths if necessary
$yii=dirname(__FILE__).'/yii/framework/yii.php';
$config=dirname(__FILE__).'/protected/config/main.php';

require_once($yii);
Yii::createWebApplication($config);
```

После этого можно в любом скрипте подключить файл `yiiapp.php` и получить
готовое приложение со всеми его возможностями:

```php
include "yiiapp.php";
$cmd = Yii::app()->db->createCommand("SELECT COUNT(*) FROM {{users}}");
$count = $cmd->queryScalar();

echo "Зарегистрированных пользователей на сайте: $count";
```

---
  - `Автор`: [R3D3](http://yiiframework.ru/forum/memberlist.php?mode=viewprofile&u=835)
