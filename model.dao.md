DAO и модели
============

Active Record в Yii довольно мощный и гибкий. Для своей гибкости, ресурсов,
при не очень большом количестве записей, он потребляет не так много,
но в некоторых случаях AR может не подходить:

- Отличное от БД хранилище данных (например, XML или Redis).
- Производительность и потребление памяти при работе с большим количеством объектов.
- Недостаточная гибкость (например, одновременная работа с несколькими хранилищами данных).

В этом уроке мы ни в коем случае не призываем отказываться от AR, а всего-лишь
рассматриваем альтернативный способ построения моделей.

Модель
------

Для примера опишем сущность «фильм» в модели `Film`. При этом данные фильма могут
храниться как в БД, так и в XML, кэше, Redis и т.д. В нашем случае, поскольку мы
работаем с БД, один объект модели `Film` соответствует одной записи в бд.

```php
class Film extends CModel {
    public $id;
    public $name;
    public $comments; // коллекция комментариев

    public function rules() {
        return array(
                array('name', 'required'),
                array('name', 'length', 'max'=>255),
        );
    }

    public function getName(){
        return $this->name;
    }

    public function getId(){
        return $this->id;
    }

    public function setName($newName){
        $this->name = $newName;
    }

    public function getComments(){
         return $this->comments;
    }
}
```

Фасад и фабрики
---------------

Далее создадим фасад `FilmManager`, реализующий все основные методы для работы
с моделью, такие как:

- getFilmById.
- getFilmsByYear.
- getLastFilms.
- updateFilm.
- и другие.

Данный класс позволяет прозрачно работать с реализованными
в отдельных классах-фабриках методами для разных
хранилищ данных (БД, XML, Sphinx и др.).

```php
class FilmManager {
    public function __construct(){

    }

    public static function factory($driver = 'DB'){
        return new 'Film'.$driver.'Manager';
    }

    /**
     *
     * @param int $id
     * @return Film
     */
    public function getFilmById($id) {
    }
}
```

Создаём фабрики:
```php
class FilmDBManager extends FilmManager {
    public function getFilmById($id) {
        $db = Yii::app()->db;
        $film_table = Film::tableName();

        $sql = "SELECT * FROM {$film_table} WHERE id=:ID LIMIT 1";
        $command=$db->createCommand($sql);
        $command->bindParam(":ID", $id);
        $row = $command->queryRow();

        $film = new Film;
        $film->id = $row['id'];
        $film->name_ru = $row['name_ru'];

        //получаем комментарии для фильма
        $film->comments = CommentsManager::getCommentsForFilm($id);

        return $film;
    }
}
```

```php
class FilmXMLManager extends FilmManager {
    public  function getFilmById($id) {
       // берём из xml данные, создаем экземпляр класса Film
       return $film;
    }
}
```

Будем работать с фабриками через фасад:

```php
// получаем фильм из БД
$film = FilmManager::factory()->getFilmById($id);
echo $film->getName();

foreach($film->getComments() as $comment){
    echo $comment->getText();
    echo $comment->getAuthor()->getName();
}

// получаем фильм из XML
$film = FilmManager::factory('XML')->getFilmById($id);
echo $film->getName();
```

В итоге имеем единый API, позволяющий получать объекты одного и того же типа,
с возможностью быстрого переключения на другое хранилище данных.

Коллекция
---------

В случае, когда метод фабрики возвращает не одну модель, а несколько,
логично возвращать не массивы, а типизированные коллекции.
Создадим свой тип коллекции для хранения фильмов:

```php
class FilmCollection extends CList {
    // общее число фильмов
    private $totalCount;

    // Задаём тип параметра Film.
    // При попытке добавления объекта другого типа
    // произойдёт ошибка.
    public function add(Film $item){
        parent::add($item);
    }

    public function setTotal($count){
        $this->totalCount = $count;
    }

    public function getTotal(){
        return $this->totalCount;
    }
}
```

---
  - `Авторы`: [pirrat](http://yiiframework.ru/forum/memberlist.php?mode=viewprofile&u=59), Sam Dark ([rmcreative.ru](http://rmcreative.ru/))
  - `Обсуждение и комментарии`: [http://yiiframework.ru/forum/viewtopic.php?f=8&t=456](http://yiiframework.ru/forum/viewtopic.php?f=8&t=456)
