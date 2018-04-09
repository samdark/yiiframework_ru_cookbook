Как написать новый рецепт
=========================

> Note|Примечание: [исходный код данного файла](https://github.com/samdark/yiiframework_ru_cookbook/blob/master/new.recipe.howto.txt)
можно использовать как основу для нового рецепта.

Сначала стоит описать кратко суть того, что будет рассказано или сделано. Можно
привести основные мысли списком:

- Как оформлять код.
- Как работать с github.
- Как установить git.
- Как добавить рецепт в общее оглавление.

> Note|Примечание: для разметки используется markdown с небольшими дополнениями.
Список терминов можно посмотреть
[в «Russian Translation Guidelines» на github](https://github.com/yiisoft/yii/wiki/Russian-Translation-Guidelines).

Как оформлять код
-----------------

Код оформляется вот так:

```php
$author = new User();
$author->postRecipe();
```

Если не указать `[php]`, то код будет без подсветки. Вместо `[php]` можно указать
`[css]`, `[javascript]`, `[html]` и другие языки.

Как установить git и настроить его для работы с github
------------------------------------------------------

Git [очень просто устанавливается на всех операционных системах](http://git-scm.com/downloads).
[Подробнее можно почитать на github](https://help.github.com/articles/set-up-git).
Начальная конфигурация также не сильно сложная.

Открываем консоль, указываем имя, под которыми будем коммитить и email,
который используется на github.

```
git config --global user.name "Your Name Here"
git config --global user.email "your_email@youremail.com"
```

Возможно понадобится [сгенерировать ключик и добавить его на github](https://help.github.com/articles/generating-ssh-keys).

В качестве визуального клиента рекомендую [SmartGit](http://www.syntevo.com/smartgit/index.html).

Как работать с github
---------------------

### Начальная настройка

- Идём в веб интерфейс github, [на страницу репозитория с рецептами](https://github.com/samdark/yiiframework_ru_cookbook).
- Жмём кнопочку «Fork».
- Идём в свой репозиторий `https://github.com/your-nickname/yiiframework_ru_cookbook`.
- Копируем адрес своего репозитория. Например, `git@github.com:your-nickname/yiiframework_ru_cookbook.git`.
- Забираем проект себе, добавляем `upstream`, ссылающийся на оргинальный репозиторий. Открываем консоль:

```
git clone git@github.com:your-nickname/yiiframework_ru_cookbook.git
git remote add upstream git@github.com:samdark/yiiframework_ru_cookbook.git
```

### Новый рецепт или правки существующего

- Открываем консоль, забираем последний код из оригинального репозитория. Делаем
  свою ветку для правок:

```
git fetch upstream
git checkout upstream/master
git checkout -b my-cool-changes
```

- Открываем файлы в любимом редакторе. Отлично справляется PhpStorm с плагином
  «markdown», если его настроить на текстовые файлы.
- Делаем необходимые правки.
- Открываем консоль в той же директории, добавляем наши правки, делаем коммит и
  отправляем в свой репозиторий:

```
git add .
git commit -m "описание ваших изменений"
git push -u origin my-cool-changes
```

- Идём в веб-интерфейс github в свой репозиторий.
- Выбираем нашу ветку используя селектор «branch:».
- Жмём кнопку «Pull Request», вводим описание и отсылаем реквест.

Через какое-то время я получаю уведомление о новых изменениях, делаю
необходимые замечания. Чтобы их исправить делаются правки тех же файлов в той
же ветки и повторяется

```
git add .
git commit -m "описание ваших изменений"
git push
```

повторно pull request делать при этом не надо.

После того, как изменения готовы к публикации, я делаю merge и они сразу же
появляются на сайте.

Как добавить рецепт в общее оглавление
-------------------------------------

Рецепт добавляется простым вписыванием его в файл
[`toc.txt`](https://github.com/samdark/yiiframework_ru_cookbook/blob/master/toc.txt).

---
  - `Автор`: Александр Макаров, Sam Dark ([rmcreative.ru](http://rmcreative.ru/)).
  - `Английский рецепт`: если это перевод, обязательно указывайте оригинал.
  - `Обсуждение и комментарии`: можно сослаться на тему с обсуждением рецепта на
    форуме.