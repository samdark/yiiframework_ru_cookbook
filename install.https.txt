HTTPS и Yii
===========

Задача: использовать в приложении настройку, включающую для определённого
пользователя использование HTTPS.

Первым делом реализуем удобное получение самой настройки из базы через
`Yii::app()->user->useHttps`. Для этого расширим [CWebUser]:
```php
class WebUser extends CWebUser {
    private $_model = null;

    function getUseHttps() {
        if($user = $this->getModel()){
            // в таблице User есть поле useHttps
            return $user->useHttps;
        }
    }

    private function getModel(){
        if($this->_model === null){
            $this->_model = User::model()->findByPk($this->id);
        }
        return $this->_model;
    }
}
```

Чтобы приложение использовало наш класс `WebUser`, необходимо сконфигурировать
компонент `user`:

```php
'user'=>array(
    'class' => 'WebUser',
    // …
),
```

> Tip|Подсказка: Точно таким же способом можно переопределить любой компонент Yii.

Далее реализуем проверку настройки и установку нужного режима работы. Для этого
создадим базовый контроллер:

```php
class BaseController extends CController {
	function init(){
        // Если параметр не пуст и текущий режим не соответствует выставленному в настройках
        if(
            !empty(Yii::app()->user->useHttps) &&
            Yii::app()->request->isSecureConnection!==Yii::app()->user->useHttps
        ){
			$schema = 'http://';
			if(Yii::app()->user->useHttps){
				$schema = 'https://';
			}

            // получим URL с нужной схемой
			$url = $schema.Yii::app()->request->serverName.Yii::app()->request->url;
			
            // перенаправим на него пользователя
			Yii::app()->request->redirect($url, true);
		}
	}
}
```

Все контроллеры нашего приложения необходимо наследовать от базового.

> Tip|Подсказка: При использовании nginx необходимо задать
`fastcgi_param HTTPS on;` для всех хостов с HTTPS.

---
  - `Автор`: Александр Макаров, Sam Dark ([rmcreative.ru](http://rmcreative.ru/))
  - `Обсуждение и комментарии`: [http://yiiframework.ru/forum/viewtopic.php?f=8&t=157](http://yiiframework.ru/forum/viewtopic.php?f=8&t=157)
