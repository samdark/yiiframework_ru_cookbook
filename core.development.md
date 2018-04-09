Удобная разработка и отладка в Yii
==================================

Yii предоставляет достаточно широкие возможности для удобной разработки и отладки.
Покажем несколько полезных приёмов.

Включаем профайлер
------------------
Для того, чтобы посмотреть, какие запросы к БД происходят и сколько времени тратится
на каждый, можно включить встроенный SQL-профайлер. Делается это настройкой компонентов
`db` и `log`.

`main.php`:
```php
'db'=>array(
		 …
		// включаем профайлер
		'enableProfiling'=>true,
		// показываем значения параметров
		'enableParamLogging' => true,
),
'log'=>array(
	'class'=>'CLogRouter',
	'routes'=>array(
		…
		array(
			// направляем результаты профайлинга в ProfileLogRoute (отображается
			// внизу страницы)
			'class'=>'CProfileLogRoute',
			'levels'=>'profile',
			'enabled'=>true,
		),
	),
),
```

Показываем сообщения внизу страницы
-----------------------------------

Кроме сообщений профайлера, внизу страницы можно показать и все остальные
отладочные сообщения:

```php
'log' => array(
	'class' => 'CLogRouter',
	'routes' => array(
		array(
			'class' => 'CWebLogRoute',
			'categories' => 'application',
			'levels'=>'error, warning, trace, profile, info',
		),
	),
),
```

Отдаём сообщения в FireBug
--------------------------

Плагин [FireBug](http://getfirebug.com/) для FireFox очень удобен для отладки
приложения. Чтобы вывести информацию в его консоль можно воспользоваться
следующей конфигурацией:

```php
'log' => array(
	'class' => 'CLogRouter',
	'routes' => array(
		array(
			'class' => 'CWebLogRoute',
			'categories' => 'application',
			'showInFireBug' => true
		),
	),
),
```

Отсылка ошибок почтой
---------------------

Если необходимо следить за возникновением ошибок, можно отправлять их себе
на почту:

```php
'log' => array(
	'class' => 'CLogRouter',
	'routes' => array(
		array(
			'class' => 'CEmailLogRoute',
			'categories' => 'error',
			'emails' => array('sam@rmcreative.ru'),
			'sentFrom' => 'error@yiiframework.ru',
			'subject' => 'Error at YiiFramework.ru'
		),
	),
),
```

Отладка bizRule RBAC
--------------------

Начиная с Yii 1.1.3 можно включить error_reporting для правил bizRule. Писать
их придётся со всеми проверками на существование параметров, зато тихих ошибок
больше не будет.

```php
'authManager' => array(
  // показываем ошибки только в режиме отладки
  'showErrors' => YII_DEBUG
),
```

Дампер для отладки кода
-----------------------
Для более удобного использования встроенного дампера можно использовать следующий компонент:

```php
<?php
class VarDumper extends CVarDumper {
    /**
     * Displays a variable.
     * This method achieves the similar functionality as var_dump and print_r
     * but is more robust when handling complex objects such as Yii controllers.
     * @param mixed variable to be dumped
     * @param integer maximum depth that the dumper should go into the variable. Defaults to 10.
     * @param boolean whether the result should be syntax-highlighted
     */
    public static function dump($var,$depth=10,$highlight=true){
        echo self::dumpAsString($var,$depth,$highlight);
    }
}
```

Отличие от стандартного [CVarDumper] всего одно — подсветка кода включена по умолчанию.

Используется так:

```php
VarDumper::dump($_SERVER);
```

Сторонние расширения
--------------------

Поддержку [FirePHP](http://www.firephp.org/) можно получить используя расширения
[yii-firephp](http://code.google.com/p/yii-firephp/) или
[firephp-logroute](http://www.yiiframework.com/extension/firephp-logroute/).


Также стоит обратить внимание на
[Yii Debug Toolbar](http://www.yiiframework.com/extension/yiidebugtb/)
и [syslogroute](http://www.yiiframework.com/extension/syslogroute/).

Полезные методы и классы
------------------------

- [Yii::log].
- [Yii::trace].
- [CVarDumper].
- [Yii::beginProfile], [Yii::endProfile].

Стоит изучить
-------------
  - [Используем несколько конфигураций в одном приложении](install.many.configs).
  - [Журналирование](/doc/guide/ru/topics.logging).
  - [Обработка ошибок](/doc/guide/ru/topics.error).

---
  - `Автор`: Александр Макаров, Sam Dark ([rmcreative.ru](http://rmcreative.ru/))
  - `Обсуждение и комментарии`: …
