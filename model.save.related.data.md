Сохранение связанных данных
===========================

Довольно часто требуется сохранить связанные данные для текущей модели. К примеру,
при сохранении модели команды сохранить её членов:

```php
// создаём команду
$team = new Team();
$team->name = 'yiiframework.ru documentation team';

// создаём члена
$member1 = new Member();
$member1->name = 'Alexander';

// создаём члена
$member2 = new Member();
$member2->name = 'Alexey';

// добавляем членов в команду
$team->members[] = $member1;
$team->members[] = $member2;

// сохраняем
$team->save();
```

Чтобы код выше работал, определим метод `afterSave` модели `Team`:

```php
class Team extends CActiveRecord{
    // …
    // выполняется сразу после сохранения Team
    protected function afterSave(){
        parent::afterSave();
        
        foreach($this->members as $member) {
            // задаём членам id команды
            $member->teamId = $this->id;
            $member->save();
        }
    }
    // …
}
```

---
  - `Автор`: Александр Макаров, Sam Dark ([rmcreative.ru](http://rmcreative.ru/))
  - `Обсуждение и комментарии`: [http://yiiframework.ru/forum/viewtopic.php?f=8&t=158](http://yiiframework.ru/forum/viewtopic.php?f=8&t=158)
