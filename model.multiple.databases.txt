Использование Active Record с несколькими БД
============================================

Иногда необходимо работать одновременно с несколькими БД. При этом не хочется
терять гибкость Active Record и работать через модели [CActiveRecord] с использованием
связей, именованных групп и прочих удобств.

Так как полностью прозрачной поддержки такой работы
[пока не намечается](http://code.google.com/p/yii/issues/detail?id=797), покажем
на примере MySQL, как можно реализовать её с относительно небольшими затратами.

Создадим две БД: `db1` и `db2` и пропишем их в конфигурации Yii:

```php
'components'=>array(
    //…
    'db'=>array(
        'class'=>'system.db.CDbConnection',
        'connectionString'=>'mysql:host=localhost;dbname=db1',
        'username'=>'root',
        'password'=>'',
        'charset'=>'utf8',
    ),

    'db2'=>array(
        'class'=>'system.db.CDbConnection',
        'connectionString'=>'mysql:host=localhost;dbname=db2',
        'username'=>'root',
        'password'=>'',
        'charset'=>'utf8',
    ),
    //…
),
```

В `db1` создадим таблицу `post`:

```sql
/* Post table */
DROP TABLE IF EXISTS `post`;
CREATE TABLE IF NOT EXISTS `post` (
  `id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
  `title` VARCHAR(255) NOT NULL,
  `text` TEXT NOT NULL,
  PRIMARY KEY  (`id`)
);
```

В `db2` — таблицу `comment`:

```sql
/* Comment table */
DROP TABLE IF EXISTS `comment`;
CREATE TABLE IF NOT EXISTS `comment` (
  `id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
  `text` TEXT NOT NULL,
  `postId` INT(10) UNSIGNED NOT NULL,
  PRIMARY KEY  (`id`)
);
```

Так как `Post` использует БД по умолчанию, никаких отличий от стандартной модели
не будет.

Займёмся моделью `Comment`. Переопределим соединение и имя таблицы:

```php
class Comment extends CActiveRecord {
    //…

    // отдаём соединение, описанное в компоненте db2
    public function getDbConnection(){
        return Yii::app()->db2;
    }

    // возвращаем имя таблицы вместе с именем БД
    public function tableName(){
         return 'db2.comment';
    }

    //…
}
```

Пробуем:
```php
Post::model()->with('comments')->findAll();
```

> Info|Информация: для Oracle имя таблицы с БД необходимо писать как
> `comment@db2`, для MSSQL как `db2.dbo.comment`. В PostgreSQL кроссбазовые
> JOIN не поддерживаются. Можно попробовать использовать схемы.


---
  - `Автор`: Sam Dark ([rmcreative.ru](http://rmcreative.ru/)), [creocoder](http://www.yiiframework.com/forum/index.php?/user/801-creocoder/)
  - `Обсуждение и комментарии`: [http://yiiframework.ru/forum/viewtopic.php?f=8&t=494](http://yiiframework.ru/forum/viewtopic.php?f=8&t=494)