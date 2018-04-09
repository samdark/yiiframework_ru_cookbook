События
=======

Родитель большинства классов Yii — [CComponent] позволяет сделать приложение
очень гибким, так как поддерживает события.

Для описания события необходимо в классе-потомке [CComponent] объявить метод,
имя которого начинается на `on`. Например, если объявить метод `onRegister`, будет
создано одноимённое событие. Метод одновременно является обработчиком по умолчанию.

Процесс работы с событием можно описать следующим образом:

  1. Объявляем некоторое событие описанием соответствующего метода.
  2. Назначаем событию один или несколько обработчиков.
  3. Событие вызывается компонентом через метод [CComponent::raiseEvent].
  4. При вызове события автоматически срабатывают все подписанные на него
методы-обработчики.

Перекрытие метода в потомке компонента
--------------------------------------

Для классов моделей вместо использования событий можно перекрывать соответствующие
методы. Это менее затратно и, в большинстве случаев, отлично работает.

Задача — после валидации модели формы UserForm получить значение поля
`fullName`(имя-фамилия) из введённых полей `firstName`(имя) и `lastName`(фамилия).

В классе [CModel], являющимся базовым классом для всех моделей, есть метод
[CModel::afterValidate], срабатывающий после успешной проверки формы. Этим можно
воспользоваться, перекрыв метод в нашей модели `UserForm`:

```php
class UserForm extends CFormModel {
    public $firstName;
    public $lastName;
    public $fullName;

    public function rules(){
        return array(
            // Имя и фамилию нужно ввести обязательно
			array('firstName, lastName', 'required'),
		);
    }

    public function afterValidate(){
        // Модель проверена и наполнена.
        // Получаем fullName:
        $this->fullName = $this->firstName.' '.$this->lastName;

        // Передаём эстафетную палочку другим обработчикам
        // данного события.
        return parent::afterValidate();
    }
}
```

Этот подход широко [используется при разработке поведений](/doc/guide/ru/extension.create)
и своих моделей Active Record.

Работа с событиями
------------------

Любому объявленному событию можно назначить обработчик. Сделать это можно несколькими
способами.

### Назначение обработчика при помощи attachEventHandler
Для назначения обработчика можно воспользоваться методом [CComponent::attachEventHandler].
Метод принимает два параметра: `$name` — имя события и `$handler` — обработчик события.
В качестве обработчика можно указать:

- Глобальную функцию: 'my_function'.
- Статический метод класса: array('имя класса', 'имя статического метода класса').
- Метод объекта: array($object, 'имя метода объекта').
- Анонимную функцию как результат
  [create_function](http://ru2.php.net/manual/en/function.create-function.php).

  Например:

```php
$component->attachEventHandler('onClick', create_function('$event', 'echo "Клик!";'));
```

- В PHP 5.3 можно использовать
  [анонимные функции](http://ru2.php.net/manual/en/functions.anonymous.php)
  без create_function:

```php
$component->attachEventHandler('onClick', function($event){
    echo "Клик!";
});
```

> Note|Примечание: В случае использования `attachEventHandler` обработчик события заносится в список
обработчиков последним.

Обработчик события всегда определяется как `function eventHandler($event){…}`,
где `$event` является экземпляром класса [CEvent]. Данный класс содержит всего
два свойства: `sender` и `handled`. В первом находится объект, вызвавший событие;
второе, путём присваивания ему значения `true`, позволяет предотвратить вызов
ещё не выполненных обработчиков события.

### Альтернативный синтаксис

Можно назначать обработчики непосредственно событиям:
```php
$component->onClick=$handler;
// или:
$component->onClick->add($handler);
```

### Использование getEventHandlers
[CComponent::getEventHandlers] — очень полезный метод. Он возвращает для переданного
параметром имени события список (объект [CList]) всех назначенных обработчиков события.

Следующий код полностью повторяет метод `attachEventHandler`:
```php
$component->getEventHandlers('onClick')->add($handler);
```

`getEventHandlers` удобно использовать в том случае, если важен порядок вызова
обработчиков. Например, добавить обработчик первым в список можно
следующим образом:

```php
$component->getEventHandlers('onClick')->insertAt(0, $handler);
```

### Удаление обработчика события
Удалить определённый обработчик события можно при помощи метода [CComponent::detachEventHandler]:

```php
$component->detachEventHandler('onClick', $handler);
```

Также можно получить список [CList] при помощи [CComponent::getEventHandlers] и
уже из него удалить обработчики.

### Другие полезные методы

- [CComponent::hasEvent] — позволяет узнать, объявлено ли указанное событие
  в компоненте.
- [CComponent::hasEventHandler] — позволяет узнать, назначены ли указанному
  событию какие-либо обработчики.

Использование существующих событий
----------------------------------

В Yii объявлено довольно большое количество различных событий и все их можно
использовать для выполнения своего кода на определённых этапах работы приложения.

Например, события
[CApplication::onBeginRequest] и [CApplication::onEndRequest], вызываемые
соответственно перед началом выполнения приложения и после его выполнения,
можно использовать для сжатия кода страницы (всё-же, делать это лучше
средствами веб-сервера):

```php
Yii::app()->onBeginRequest = function($event){
    return ob_start("ob_gzhandler");
};

Yii::app()->onEndRequest = function($event){
    return ob_end_flush();
};
```

> Info|Информация: чтобы найти все определённые в Yii события, ищите «`function on`»
> в директории `framework`.

Назначение обработчиков через конфигурацию
------------------------------------------
Для компонентов Yii есть возможность назначить обработчик события через файл
конфигурации.

Например, назначить обработчик событию [CMessageSource::onMissingTranslation],
вызываемому при отсутствии перевода сообщения через [Yii::t], можно следующим
образом:

`main.php`:
```php
…
'components' => array(
	…
	// Класс компонента messages — CPhpMessageSource
	'messages' => array(
		'onMissingTranslation' => array('MyEventHandler', 'handleMissingTranslation'),
	),
	…
)
…
```

Сам класс будет выглядеть так:

```php
class MyEventHandler {
	public static function handleMissingTranslation($event){
		// обрабатываем отсутствие перевода
	}
}
```

Примерно так же можно поступить с событиями приложения. Отличие в том, что
используется первый уровень вложенности в массиве конфигурации.

`main.php`:
```php
'onBeginRequest' => function($event){
	return ob_start();
},
'onEndRequest' => function($event){
	$out=ob_get_flush();
	file_put_contents($fileName, $out);
	return $out;
}
```

Своё событие
------------

Покажем создание своего события и его вызов на примере.

Задача — отослать почту при добавлении комментария к записи блога.

```php
class Post extends CModel {
    function addComment(Comment $comment){
        // добавляем в блог новый комментарий
        // …

        // Создаём экземпляр потомка CEvent
        $event = new CModelEvent($this);
        // Вызываем событие
        $this->onNewComment($event);
        return $event->isValid;
    }

    // описываем событие onNewComment
    public function onNewComment($event) {
        // Непосредственно вызывать событие принято в его описании.
        // Это позволяет использовать данный метод вместо raiseEvent
        // во всём остальном коде.
        $this->raiseEvent('onNewComment', $event);
    }
}

// Класс-оповещатель
// Рассылает почту при различных событиях
class Notifier {
    function comment($event){
        $post = $event->sender;
        foreach($post->comments as $comment){
            // отправляем почту каждому подписавшемуся комментатору
        }
    }
}
```

Назначаем оповещателя обработчиком:

```php
$postModel = Post::model()->findByPk(10);
$notifier = new Notifier();
$postModel->onNewComment = array($notifier, 'comment');
```

Теперь при вызове

```php
$postModel->addComment($comment);
```

будет выполнен метод `$notifier->comment`, который оповестит комментаторов о новом
сообщении.

---
  - `Автор`: Александр Макаров, Sam Dark ([rmcreative.ru](http://rmcreative.ru/))
  - `Обсуждение и комментарии`: [http://yiiframework.ru/forum/viewtopic.php?f=8&t=127](http://yiiframework.ru/forum/viewtopic.php?f=8&t=127)
