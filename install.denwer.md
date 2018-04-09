Yii и Denwer
============

[Denwer](http://www.denwer.ru/) является довольно популярным средством для
разработки на PHP. Остановимся на некоторых моментах, полезных при работе с Yii.

Ниже будет предполагаться, что Denwer установлен на диск `Z:` в директорию `web`
и при его запуске создаётся виртуальный диск `W:`.

Консоль
-------
Для того, чтобы использовать PHP из консоли:

- К переменной окружения PATH добавляем полный путь к php.exe от корня виртуального
   диска `W:`. Для английской Windows 7 надо зайти в System Properties → Advanced →
   Environment Variables → System Variables → Path → Edit и дописать `;W:\usr\local\php5`.
- Отредактируйте `W:\usr\local\php5\php.ini` так, чтобы все пути были абсолютными
   от корня виртуального диска Denwer.

```
extension_dir = "W:\usr\local\php5\ext\"
session.save_path = "W:\tmp"
```

- Иногда требуются некоторые dll (об этом будет написано в выводимых ошибках).
   Их необходимо найти и положить в папку PHP.

Проблемы с UTF-8
----------------
Поправим кодировку для отдаваемых данных по умолчанию.
В `W:\usr\local\php5\php.ini` прописываем:

```
default_charset = "utf-8"
```

Поправим кодировку для работы с MySQL.
В `W:\usr\local\mysql5\my.cnf`:

```
default-character-set = utf8
init-connect = "set names utf8"
```

---
  - `Автор`: Александр Макаров, Sam Dark ([rmcreative.ru](http://rmcreative.ru/))
  - `Обсуждение и комментарии`: [http://yiiframework.ru/forum/viewtopic.php?f=8&t=510](http://yiiframework.ru/forum/viewtopic.php?f=8&t=510)