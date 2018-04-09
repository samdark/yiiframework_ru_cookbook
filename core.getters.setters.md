Геттеры и сеттеры в Yii
=======================

В большинстве классов Yii, наследуемых от [CComponent]
(среди них модель и контроллер), поведение несколько отличается от
стандартного PHP и похоже больше на свойства C#.

```php
class MyModel extends CModel {
	function getReadwrite(){
		//…
	}

	function setReadwrite(){
		//…
	}

    function getReadonly(){
		//…
	}
	
    function setWriteonly(){
        //…
    }
}
```

Такой класс определяет три свойства: readwrite, readonly и writeonly:

```php
$model = new MyModel();

$model->readwrite = 'мы можем сюда писать';
//и читать
echo $model->readwrite;

//отсюда можно только читать
echo $model->readonly;

$model->writeonly = 'а сюда только писать';
```

---
  - `Автор`: Александр Макаров, Sam Dark ([rmcreative.ru](http://rmcreative.ru/))
  - `Обсуждение и комментарии`: [http://yiiframework.ru/forum/viewtopic.php?f=8&t=126](http://yiiframework.ru/forum/viewtopic.php?f=8&t=126)
