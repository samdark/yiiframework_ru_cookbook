Свои сообщения для ошибок валидации
===================================

Валидаторы, наследуемые от [CValidator], содержат свойство `message`. Вы можете
установить его для изменения сообщения определённого правила:

```php
class Post extends CActiveRecord
{
    public function rules()
    {
        return array(
            array('title, content', 'required',
                  'message'=>'Введите значение {attribute}.'),
                  // другие правила валидации
        );
    }
}
```

`{attribute}` заменяется именем поля в [CRequiredValidator] при возникновении в данном поле ошибки.

> Tip|Подсказка: для CStringValidator (т.е. правила `length`) вместо `message`
  используются свойства `tooLong` для слишком длинных полей и `tooShort` для
  слишком коротких. Для `CNumberValidator` (`number`), соответственно, используются
  `tooBig` и `tooSmall`.

---
  - `Оригинал`: [http://www.yiiframework.com/doc/cookbook/1/](http://www.yiiframework.com/doc/cookbook/1/)
  - `Перевод`: Александр Макаров, Sam Dark ([rmcreative.ru](http://rmcreative.ru/))
  - `Дополнения`: [Ekstazi](http://yiiframework.ru/forum/memberlist.php?mode=viewprofile&u=548)
  - `Обсуждение и комментарии`: [http://yiiframework.ru/forum/viewtopic.php?f=8&t=11](http://yiiframework.ru/forum/viewtopic.php?f=8&t=11)
