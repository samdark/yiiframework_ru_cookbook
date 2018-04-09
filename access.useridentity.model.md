Используем модель как UserIdentity
==================================

В разделе «[аутентификация](http://yiiframework.ru/doc/blog/ru/prototype.auth)»
руководства по созданию блога на Yii описывается использование интерфейса
[[IUserIdentity]] и реализующего его класса [[CUserIdentity]]. В данном рецепте
мы рассмотрим немного отличающийся от предствленного в руководстве подход. На наш
взгляд он является более удобным.

Основная идея — совместить модель пользователя (обычно это Active Record с именем
`User`) с `UserIdentity`. Поскольку в [[CUserIdentity]] нет никакого сложного
функционала, отказаться от него просто. Вместо этого будем использовать интерфейс:

```php
class User extends CActiveRecord implements IUserIdentity
{
	private $_id;
	private $_name;
	private $_isAuthenticated=false;
	private $_state=array();

	public static function model($className=__CLASS__)
	{
		return parent::model($className);
	}

	public function tableName()
	{
		return 'user';
	}

	public function rules()
	{
		return array(
			array('email','required','on'=>'login,insert'),
			array('password','required','on'=>'login'),
			array('email','email'),
		);
	}

	public function attributeLabels()
	{
		return array(
			'email'=>'а­аЛаЕаКббаОаНаНаАб аПаОббаА',
			'password'=>'ааАбаОаЛб',
		);
	}

	public function authenticate()
	{
		$model=self::model()->findByAttributes(array('email'=>$this->email));

		if($model===null || $model->password!==sha1($this->password))
		{
			$this->addError('password','ааОаЛбаЗаОаВаАбаЕаЛб б баАаКаОаЙ баЛаЕаКббаОаНаНаОаЙ аПаОббаОаЙ аНаЕ аНаАаЙаДаЕаН аИаЛаИ аНаЕ аВаЕбаНбаЙ аПаАбаОаЛб.');
			return false;
		}

		if(!$model->is_confirmed)
		{
			$this->addError('email','ааЕбаЕаД аВбаОаДаОаМ аНаЕаОаБбаОаДаИаМаО аПаОаДбаВаЕбаДаИбб аАаДбаЕб баЛаЕаКббаОаНаНаОаЙ аПаОббб.');
			return false;
		}

		$this->_id=$model->id;
		$this->_name=$model->email;
		$this->_isAuthenticated=true;
		$this->setState('locality_id',$model->locality_id);

		return true;
	}

	protected function beforeSave()
	{
		if(!parent::beforeSave())
			return false;

		$this->code=$this->isNewRecord ? uniqid() : null;

		return true;
	}

	protected function afterSave()
	{
		$this->_id=$this->id;
		$this->_name=$this->email;
		$this->_isAuthenticated=true;
	}

	public function getId()
	{
		return $this->_id;
	}

	public function getName()
	{
		return $this->_name;
	}

	public function getPersistentStates()
	{
		return $this->_state;
	}

	public function setPersistentStates($states)
	{
		$this->_state=$states;
	}

	public function getIsAuthenticated()
	{
		return $this->_isAuthenticated;
	}

	public function getState($name,$defaultValue=null)
	{
		return isset($this->_state[$name]) ? $this->_state[$name] : $defaultValue;
	}

	public function setState($name,$value)
	{
		$this->_state[$name]=$value;
	}

	public function clearState($name)
	{
		unset($this->_state[$name]);
	}
}
```

---
  - `Авторы`: [creocoder](http://yiiframework.ru/forum/memberlist.php?mode=viewprofile&u=722), Sam Dark ([rmcreative.ru](http://rmcreative.ru/))
  - `Обсуждение и комментарии`: [http://yiiframework.ru/forum/](http://yiiframework.ru/forum/)
