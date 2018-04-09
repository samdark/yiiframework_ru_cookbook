Как загрузить файл используя модель
===================================

Во-первых, объявим атрибут (свойство) в модели, которое будет содержать имя
файла. Также объявим правила валидации для загружаемого файла, чтобы разрешить
загружать файлы определенного типа.

```php
class Item extends CActiveRecord {
    public $image;
    // другие свойства

    public function rules(){
        return array(
            //устанавливаем правила для файла, позволяющие загружать
            // только картинки!
            array('image', 'file', 'types'=>'jpg, gif, png'),
        );
    }
}
```

Затем в контроллере определяем метод, который будет выводить форму
и принимать данные из неё:
```php
class ItemController extends CController {
    public function actionCreate(){
        $model=new Item;
        if(isset($_POST['Item'])){
            $model->attributes=$_POST['Item'];
            $model->image=CUploadedFile::getInstance($model,'image');
            if($model->save()){
                $path=Yii::getPathOfAlias('webroot').'/upload/'.$model->image->getName();
                $model->image->saveAs($path);
                // перенаправляем на страницу, где выводим сообщение об
				// успешной загрузке
            }
        }
        $this->render('create', array('model'=>$model));
    }
}
```

Если происходит загрузка нескольких файлов, код будет таким:
```php
if($model->save()){
    foreach($model->image as $file){
        $file->saveAs('path/to/localFile');
    }
    // перенаправляем на страницу, где выводим сообщение об
    // успешной загрузке
}
```

И, наконец, создаём представление с формой:

```php
<?php echo CHtml::form('','post',array('enctype'=>'multipart/form-data')); ?>
…
<?php echo CHtml::activeFileField($model, 'image'); ?>
…
<?php echo CHtml::endForm(); ?>
```

> Note|Примечание: При загрузке файла, естественно, можно проверять не только тип файла, но также
размер, и другие параметры. Вот пример правила, которое разрешает загрузку
картинок размером не больше 1 Мб:

>```php
public function rules(){
    return array(
        // Максимальный и минимальный размер указываем в байтах.
        array('image', 'file', 'types'=>'jpg, gif, png', 'maxSize' => 1048576),
    );
}
```

>Подробнее о доступных правилах валидации можно прочитать в API
класса [CFileValidator].

---
  - Основан на: [http://www.yiiframework.com/doc/cookbook/2/](http://www.yiiframework.com/doc/cookbook/2/)
  - Перевод и примечание: [Pirrat](http://yiiframework.ru/forum/memberlist.php?mode=viewprofile&u=59)
