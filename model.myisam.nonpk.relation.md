Связь не по первичному ключу в MyISAM
=====================================

При использовании InnoDB или другой базы с правильно выставленными внешними ключами,
Yii отлично определяет нужные поля для связей типа `HAS_MANY`. Если же используется
MyISAM, в котором поддержки внешних ключей нет, или view, необходимо задать их в коде:

```php
class MyModel extends CActiveRecord
{
    public function getTableSchema()
    {
        $table = parent::getTableSchema();

        $table->columns['fk_column_name']->isForeignKey = true;
        $table->foreignKeys['fk_column_name'] = array('MyRelatedModel', 'related_model_fk');

        return $table;
    }
}
```

Полезные ссылки
---------------

- [Adding Foreign Keys Manually](http://danaluther.blogspot.com/2010/06/adding-foreign-keys-manually.html)

---
  - `Авторы`: [Dana Luther](http://www.yiiframework.com/forum/index.php?/user/4032-dana/), Александр Макаров, Sam Dark ([rmcreative.ru](http://rmcreative.ru/))
  - `Обсуждение и комментарии`: [http://yiiframework.ru/forum/viewtopic.php?f=8&t=2540](http://yiiframework.ru/forum/viewtopic.php?f=8&t=2540)
