Вывод CStarRating в GridView
============================

В качестве вступления: На сайте есть система комментариев с рейтингом (выставление
оценок при помощи виджета [CStarRating]). Необходимо в [CGridView] отображать
выставленный с каждым комментарием рейтинг "звёздочками".

Для этого нам потребуется в представлении, например, `admin.php` в значение
ключа `value` написать:

```php
$this->grid->controller->widget("CStarRating", array(
	"name" => $data->id,
	"id" => $data->id,
	"value" => $data->rating,
	"readOnly" => true,
), true);
```

Полный вид получится примерно следующим:

```php
array(
	'name' => 'rating',
	'type' => 'raw',
	'value'=>'$this->grid->controller->widget("CStarRating", array(
		"name" => $data->id,
		"id" => $data->id,
		"value" => $data->rating,
		"readOnly" => true,
	), true)',
	'headerHtmlOptions' => array('style' => 'width:85px;'),
	'htmlOptions' => array('class' => 'rating-block'),
	'filter' => false,
	'sortable' => false,
),
```

Ну и чтобы после поиска, сортировки или фильтрации не отваливался вид (вместо
звёзд в этом случае отображается набор радиобатонов) необходимо добавить
следующий код:

```php
'afterAjaxUpdate'=>"function() {
	jQuery('.rating-block input').rating({'readOnly':true});
}",
```

В итоге должно получится как-то так:
```php
$this->widget('zii.widgets.CGridView', array(
	'id'=>'users-comments-grid',
	'dataProvider'=>$model->search(),
	'filter'=>$model,
	'afterAjaxUpdate'=>"function() {
		jQuery('.rating-block input').rating({'readOnly':true});
	}",
	'columns'=>array(
		array(
			'name' => 'message',
			'sortable' => false,
		),
		array(
			'name' => 'rating',
			'type' => 'raw',
			'value'=>'$this->grid->controller->widget("CStarRating", array(
				"name" => $data->id,
				"id" => $data->id,
				"value" => $data->rating,
				"readOnly" => true,
			), true)',
			'headerHtmlOptions' => array('style' => 'width:85px;'),
			'htmlOptions' => array('class' => 'rating-block'),
			'filter' => false,
			'sortable' => false,
		),
	),
));
```

Стоит изучить
-------------

 - [CStarRating](http://www.yiiframework.com/doc/api/CStarRating).
 - [CGridView](http://www.yiiframework.com/doc/api/CGridView).

---
 - `Автор`: Сергей Жолобов, Xpycm ([monoray.ru](http://monoray.ru/)).