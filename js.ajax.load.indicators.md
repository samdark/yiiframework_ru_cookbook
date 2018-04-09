Индикаторы AJAX-загрузки
========================

Используя Yii довольно просто использовать [ajax](http://ru.wikipedia.org/wiki/Ajax) запросы.
Чаще всего необходимо показать пользователю, что запрос находится в процессе
выполнения.

Приведём простой и красивый пример, как этого достичь. Загружаемый элемент
будем отображать с прозрачностью в 80% и картинкой, показывающей, что идёт
загрузка.

Плюс решения в том, что не придётся добавлять дополнительную разметку.

При выполнении запроса будем добавлять класс `.loading` к обновляемому элементу.
После выполнения запроса класс будем убирать.

Добавляем в наш файл отображения:

```php
<?php
echo CHtml::form();
echo CHtml::ajaxButton (
    'DoAjaxRequest', // заголовок
    '', // url запроса
    array (
        'beforeSend' => 'function(){
            $("#myDiv").addClass("loading");
         }',
        'complete' => 'function(){
            $("#myDiv").removeClass("loading");
        }',
    )
);
echo CHtml::endForm();
?>
```

Добавим немного CSS к нашим стилям:

```css
div.loading {
    background-color: #eee;
    background-image: url('loading.gif');
    background-position: center center;
    background-repeat: no-repeat;
    opacity: 1;
}

div.loading * {
    opacity: .8;
}
```

---
 - `Оригинал`: [http://www.yiiframework.com/doc/cookbook/46/](http://www.yiiframework.com/doc/cookbook/46/)
 - `Перевод`: Александр Макаров, Sam Dark ([rmcreative.ru](http://rmcreative.ru/))
 - `Обсуждение`: [http://yiiframework.ru/forum/viewtopic.php?f=8&t=205](http://yiiframework.ru/forum/viewtopic.php?f=8&t=205)
