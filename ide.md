Настройка IDE для работы с Yii
==============================

NetBeans
--------

### Плагин для Yii

Добавляет специфичные для Yii возможности навигации, дополнения кода и другие полезности. [Забрать можно с официальной
страницы плагина](http://plugins.netbeans.org/plugin/47246/php-yii-framework-netbeans-phpcc).

### Дополнение кода

1. Подключить Yii, если он не является частью проекта.
  - `File → Project properties → PHP Include Path`.
  - Указываем путь до директории `framework`.
2. Исключить из индексации `yiilite.php`.
  - `Tools → Options → Miscellaneous → Files`.
  - `Files Ignored by the IDE`.
  - Добавляем в самое начало `^(yiilite\.php|CVS|SCCS|…`.
  - Перезапускаем NetBeans.
3. Исключить из индексации динамически генерируемые файлы.
  - `File → Project properties → Ignored Folders → Add Folder`.
  - Добавляем `assets`, `framework/cli/views` и `protected/runtime`.
4. Если используется PHPUnit, подключить его:
  - `File → Project properties → PHP Include Path`.
  - Указываем путь до директории `PHPUnit`.

- Дополнение кода: `Ctrl+Space`.
- Параметры метода: `Ctrl+P`.

### Тестирование

Для запуска функциональных и модульных тестов Yii нужно установить соответственно
SeleniumRC и PHPUnit.


  1. PHPUnit.
    - Устанавливаем [по инструкции на официальном сайте](http://www.phpunit.de/manual/3.0/en/installation.html).
    - В IDE: `Tools → Options → PHP → Unit Testing`.
    - Указываем путь к PHPUnit (для Windows это путь к `phpunit.bat`).
  2. Устанавливаем SeleniumRC из плагинов NetBeans:
    - `Tools → Plugins → Available Plugins`.
    - Устанавливаем `Selenium Module for PHP`.
  3. Указываем директорию с тестами:
    - `File → Project properties → Sources`.
    - `Test Folder` выставляем в `путь_до_проекта/protected/tests`.

  - Для запуска всех тестов в проекте используем `Alt+F6`.
  - Для запуска отдельного теста — `Shift+F6`.
  - Для получения отчёта по покрытию кода тестами щёлкаем правой кнопкой мыши по проекту и выбираем `Code Coverage`.

### Отладка
  1. Устанавливаем [Xdebug](http://www.xdebug.org/docs/install).
  2. В `php.ini` добавляем:

```
zend_extension = "C:\xampp\php\ext\php_xdebug.dll"
xdebug.remote_enable = 1
xdebug.remote_handler = "dbgp"
xdebug.remote_host = "localhost"
xdebug.remote_port = 9000
```

Отладку можно начать нажав `Ctrl-F5`.

Для использования своих точек останова идём в `Tools → Options → PHP → вкладка Debugging` и
снимаем галочку с `Stop at First Line`.

### Полезные ссылки
  - [Переносим NetBeans на флэшке](http://rmcreative.ru/blog/post/perenosim-netbeans-na-fleshchke).
  - [Макросы в NetBeans](http://rmcreative.ru/blog/post/makrosy-v-netbeans).
  - [Zen HTML для NetBeans](http://rmcreative.ru/blog/post/zen-html-dlja-netbeans) и [апдейт](http://rmcreative.ru/blog/post/netbeans-zen-html-1.1).
  - [Шаблоны кода в NetBeans](http://rmcreative.ru/blog/post/shchablony-koda-v-netbeans).

PhpStorm
--------

### YiiStorm

YiiStorm — плагин для PhpStorm, добавляющий специфичные для Yii возможности навигации, дополнения кода и другие полезные
штуки. [Установить можно с официального сайта плагина](http://mazx.ru/).

### Дополнение кода
  1. Исключаем из индекса `yiilite.php`:
    - `File → Settings → IDE Settings → File Types`.
    - В `Ignore files and folders` добавляем `yiilite.php`.
  2. Исключаем «лишние» директории, указываем ресурсы.
    - `File → Settings → Project settings → Directories`.
    - Помечаем `framework/cli/views`, `protected/runtime` и `assets` как `excluded`.
    - Помечаем корень сайта как `resource root`.
  3. Указываем путь к PHP.
    - `File → Settings → Project settings → PHP → PHP Home`.
  4. Подключаем Yii, если он не является частью проекта.
  - `File → Settings → Project settings → PHP → PHP Home → Add`.
  - Указываем путь до директории `framework`.
  5. Если используется PHPUnit, подключить его:
  - `File → Settings → Project settings → PHP → PHP Home → Add`.
  - Указываем путь до директории `PHPUnit`.

  - Дополнение кода: `Ctrl+Space`.
  - Параметры метода: `Ctrl+Q`.

### Тестирование

Для запуска модульных тестов Yii нужно установить PHPUnit.

1. PHPUnit.
    - Устанавливаем [по инструкции на официальном сайте](http://www.phpunit.de/manual/3.0/en/installation.html).
    - В IDE: `Run → Edit configurations`.
    - Жмём на плюсик.
    - `Name`: что угодно.
    - `Test`: в зависимости от того, что тестировать выбираем нужное и указываем путь.
    - `Use XML configuration file`: путь до `phpunit.xml`. Обычно `путь_до_приложения/protected/tests/phpunit.xml`.

  - Для запуска тестов используем `SHIFT+F10`.

### Полезные ссылки
  - [Портативная Web IDE](http://rmcreative.ru/blog/post/portativnaja-web-ide).
  - [Yii, автокомплит для Yii::app](http://rmcreative.ru/blog/post/yii--avtokomplit-dlja-yiiapp).

---
 - `Использованы материалы` [NetBeans IDE and Yii projects](http://www.yiiframework.com/doc/cookbook/83/).
 - `Автор`: Александр Макаров, Sam Dark ([rmcreative.ru](http://rmcreative.ru/)).
 - `Обсуждение и комментарии`: [http://yiiframework.ru/forum/viewtopic.php?f=8&t=1495](http://yiiframework.ru/forum/viewtopic.php?f=8&t=1495).
