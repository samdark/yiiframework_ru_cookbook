Мультиязычные модели
====================

Довольно часто возникает необходимость работать с содержимым проекта
на нескольких языках. В Yii при использовании Active Record это
можно реализовать очень красиво.

Для этого добавим к таблице модели поле для хранения двухсимвольного идентификатора
языка записи `lang` типа `char(2)`. 

Далее в модели зададим условие по умолчанию и реализуем именованное условие `lang`.


> Tip|Подсказка: Подробнее об именованных условиях можно прочитать в [руководстве по
                 Active Record](/doc/guide/ru/database.ar).

```php
class Post extends CActiveRecord {
    // параметры, применяемые по умолчанию
    public function defaultScope() {
        return array(
            'condition' => "lang='".Yii::app()->language."'",
        );
    }

    // именованное условие с параметром
    public function lang($lang){
        $this->getDbCriteria()->mergeWith(array(
            'condition' => "lang='$lang'",
        ));
        return $this;
    }
}
```


После этого, можно использовать следующий код:
```php
// выбираем все записи с языком, установленным в данный момент в приложении
$posts = Post::model()->findAll();
// выбираем все записи на английском
$posts = Post::model()->lang('en')->findAll();
```

---
  - `Автор`: Александр Макаров, Sam Dark ([rmcreative.ru](http://rmcreative.ru/))
  - `Обсуждение и комментарии`: [http://yiiframework.ru/forum/viewtopic.php?f=8&t=480](http://yiiframework.ru/forum/viewtopic.php?f=8&t=480)