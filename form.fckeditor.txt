Как подключить FCKeditor
========================

Для начала вам потребуется скачать последний релиз
[FCKeditor](http://www.fckeditor.net/download). Разархивируйте скачанный файл и
поместите его в папку `fckeditor` (которую предварительно надо создать в корневой
папке приложения). Далее необходимо скачать расширение Yii
[fckeditor-integration](http://www.yiiframework.com/extension/fckeditor-integration/)
и разархивировать его в папку `protected/extensions/fckeditor`.

Теперь в том месте где вы хотите использовать FCKeditor разместите следующий код:

```php
<?php $this->widget('ext.fckeditor.FCKEditorWidget', array(
  "model"=>$pages,
  "attribute"=>'content',
  "height"=>'400px',
  "width"=>'100%',
  "toolbarSet"=>'Basic',
  "fckeditor"=>Yii::app()->basePath."/../fckeditor/fckeditor.php",
  "fckBasePath"=>Yii::app()->baseUrl."/fckeditor/",
  "config" => array(
    "EditorAreaCSS"=>Yii::app()->baseUrl.'/css/index.css',),
    # http://docs.fckeditor.net/FCKeditor_2.x/Developers_Guide/Configuration/Configuration_Options
  )
);?>
```

 - `model` — экземпляр модели который будет связан с расширением.
 - `attribute` — название атрибута через который будем связывать.
 - `fckeditor` — путь к fck-редактору.
 - `fckBasePath` — адрес к редактору который будет загружен через фрейм.
 - `config` — большинство параметров в fckconfig.js могут быть изменены с помощью
 конфигурации виджета.
 
---
 - `Оригинал`: [http://www.yiiframework.com/doc/cookbook/4/](http://www.yiiframework.com/doc/cookbook/4/)
 - `Перевод`: [Ozzy](http://yiiframework.ru/forum/memberlist.php?mode=viewprofile&u=54), [dbhelp.ru](http://dbhelp.ru/)
 - `Обсуждение`: [http://yiiframework.ru/forum/viewtopic.php?f=8&t=42](http://yiiframework.ru/forum/viewtopic.php?f=8&t=42)
