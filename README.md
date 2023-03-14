

# 02 - PHP OOP, Namespaces and PSR-4 Autoload Quick Start

[شاهد الفديو](https://www.youtube.com/watch?v=9ZNx-mFyB5g&list=PL13Ag2mfco64zMLcFjPb5GVWCu-OAjTrx&index=2)


ال classes بتحتوي علي حاجه اسمها ال properties
وال properties لها ال access modifier الخاص بها

ال properties بتكون عبارة عن variable

انواع ال access modifier
* public : accessible لهذا ال property من أي مكان حتي من خارج ال class
* protected : accessible لهذا ال property يكون فقط من هذا ال class او ابناؤها
* private : accessible لهذا ال property يكون فقط من هذا ال class وليس من ابناؤها

 _mothods_ : عبارة عن فانكشن ممكن امررها ك property لهذا ال class وبتاخد access modifier عادي (public, protected, private)

const : دي بتاخد قيمة ثابته وبتاخد access modifier عادي ( protected, private)
وال default هو public

```php
class Person 
{
    const MALE = 'm';
    const FEMALE = 'f';

    public $name = 'mohamed samir';
    protected $gender ;
    private $age ;
    
    public static setAge($age){
        $this->age = $age;
        return $this;
    }
    public static setGender($gender){
        $this->gendePr = $gender;
        return $this;
    }
}
```

يمكن استخدام ال constructor لتغيير قيمة property معينة عند التشغيل
نستخدمها في حالة ان القيمة هنرجعها من كلاس او من فانكشن أو هتحتاج عمليات معينة . لكن في حالة انها قيمة عادية . يمكن نضيفها ليها عادي زي قيمة mohamed samir
وممكن استخدامها علشان نرجع قيمة من الكلاس نفسه عندس استدعاؤه

```php
class Person 
{
    const MALE = 'm';
    const FEMALE = 'f';

    public $name = 'mohamed samir';
    public $time;
    protected $gender ;
    private $age ;

    public function __construct($name){
        $this->name = $name;
        $this->time = time();
        
    }
}
```

زي ما في static properties (const) في ايضا static method (static)

```php
class Person 
{
    const CODE = "+20";

    public static $country ;
    public static $phone ;

    public static function setCountry($country,$phone){
        self::$country = $country;
        self::$phone = self::MALE . $phone;
    }
}
```
لاحظ ان ال static لا يصلح استخدام بداخلها ال _this_ variable لأن ال _this_ دايما referance لل object وانا في حالة ال static لا اتعامل مع ال object انا بتعامل مع ال class نفسه 
بستخدم البديل هنها وهو ال **self** keyword

> لاحظ ان ال const لايمكن ان اغير قيمته لكن ال static يمكن أن اغير قيمته

### التعامل مع كلاس من داخل كلاس اخر في فايل اخر

```http
app.php
```
```php
class Person 
{
    const MALE = 'm';
    const FEMALE = 'f';
    const CODE = "+20";

    public $name = 'mohamed samir';
    protected $gender ;
    private $age ;
    public $time;
    
    public static $country ;
    public static $phone ;
    

    public function __construct($name){
        $this->time = time();
    }
    
    public static setAge($age){
        $this->age = $age;
        return $this;
    }
    public static setGender($gender){
        $this->gendePr = $gender;
        return $this;
    }
    public static function setCountry($country,$phone){
        self::$country = $country;
        self::$phone = self::MALE . $phone;
    }
}
```
<br />
<br />

```http
app.php
```
```php
// require './Person.php';
// include './Person.php';
// require __DIR__ . '/Person.php';
include __DIR__ . '/Person.php';

$person = new Person ;
$person->name = 'mohamed salama';
// $person->age = 10; // wrong

// Person::$country = 'palastine';
$person::$country = 'palastine';
```
> لاحظ ان ال `__DIR__` تكون magic constance لانها قيمة ثابته ولكنها متغيرة في نفس الوقت

#### توضيح فكرة ال static بصورة اوضح


```http
app.php
```
```php

include __DIR__ . '/Person.php';

$person = new Person ;
$person2 = new Person ;

$person->name = 'mohamed salama';


$person::$country = 'palastine';
$person2::$country = 'canada';

echo $person::$country; // canada
echo $person2::$country; // canada
echo Person::$country;  // canada

echo Person::MALE;  // m
```
> ال const هو static لكن مش بقدر اغير قيمته


#### توضيح فكرة ال namespace في ال php


```http
/A/Person.php
```
```php
namespace A;

define('ajyal',true);
const LARAVEL = 'Laravel A';

function hello(){
    echo 'Hello A';
}

class Person 
{
    const MALE = 'm';
    const FEMALE = 'f';
    public $name;
    protected $gender ;
    private $age ;
    public function __construct(){
        echo __class__ ;
    }
    public static setAge($age){
        $this->age = $age;
        return $this;
    }
    public static setGender($gender){
        $this->gendePr = $gender;
        return $this;
    }
    
}
```
<br>
<br>

```http
/B/Person.php
```
```php
namespace B;

// define('ajyal',true); // wrong
const LARAVEL = 'Laravel B';

function hello(){
    echo 'Hello B';
}

class Person 
{
    const MALE = 'm';
    const FEMALE = 'f';
    public $name;
    protected $gender ;
    private $age ;
    public function __construct(){
        echo __class__ ;
    }
    public static setAge($age){
        $this->age = $age;
        return $this;
    }
    public static setGender($gender){
        $this->gendePr = $gender;
        return $this;
    }
    
}
```
<br>
<br>

```http
app.php
```
```php

include __DIR__ . '/A/Person.php';
include __DIR__ . '/B/Person.php';

$person = new \A\Person ;
$person2 = new \B\Person ;

$person->name = 'mohamed salama';


$person::$country = 'palastine';
$person2::$country = 'canada';

echo $person::$country; // palastine
echo $person2::$country; // canada
echo \A\Person::$country;  // palastine

echo $person::MALE;  // m
```

وكمان ممكن نستخدمه ك extend


> لاحظ ان ال define function بتكون دائما في ال global namespace وده طبعا الفرق بينها وبين ال const
> اذا الملف اللي انا موجود فيه مش مكتوب ليه namespace فانا بكون موجود في ال global namespace

<br>
<br>

```http
app.php
```
```php
namespace A;

include __DIR__ . '/A/Person.php';
include __DIR__ . '/B/Person.php';

$person = new Person ;
$person2 = new \B\Person ;

$person->name = 'mohamed salama';


$person::$country = 'palastine';
$person2::$country = 'canada';

echo $person::$country; // palastine
echo $person2::$country; // canada
echo Person::$country;  // palastine

echo $person::MALE;  // m
```

وكمان ممكن نستخدمه ك extend

> انا حاليا في نفس ال namespace الخاص بال Person A 
> فبستدعية مباشرة
> لاحظ انني معاه في ال namespace وليس المسار

<br>
<br>

#### توضيح فكرة ال namespace في ال php باستخدام ال use
```http
app.php
```
```php
namespace A;

include __DIR__ . '/A/Person.php';
include __DIR__ . '/B/Person.php';

use \B\Person.php as PersonB;

$person = new Person ;
$person2 = new PersonB ;

$person->name = 'mohamed salama';


$person::$country = 'palastine';
$person2::$country = 'canada';

echo $person::$country; // palastine
echo $person2::$country; // canada
echo Person::$country;  // palastine

echo $person::MALE;  // m
```

وكمان ممكن نستخدمه ك extend

<br>
<br>

```http
/A/Person.php
```
```php
namespace A;

use \B\Person.php as PersonB

define('ajyal',true);
const LARAVEL = 'Laravel A';

function hello(){
    echo 'Hello A';
}


class Person extends PersonB
{
    const MALE = 'm';
    const FEMALE = 'f';
    public $name;
    protected $gender ;
    private $age ;
    public function __construct(){
        echo __class__ ;
    }
    public static setAge($age){
        $this->age = $age;
        return $this;
    }
    public static setGender($gender){
        $this->gendePr = $gender;
        return $this;
    }
    
}
```

#### عمل use ل function
```http
app.php
```
```php
namespace A;

include __DIR__ . '/A/Person.php';
include __DIR__ . '/B/Person.php';

use \B\Person.php as PersonB;
use function \B\hello() as helloB();
```

#### عمل use ل const
```http
app.php
```
```php
namespace A;

include __DIR__ . '/A/Person.php';
include __DIR__ . '/B/Person.php';

use const \B\LARAVEL as LARAVELB;
```

>لاحظ انه في php في حالة ان ال function او ال const غير موجودين في ال namespace سيتم استدعاؤهم من ال global namespace
> لاحظ ان هذه القاعدة لا تنطبق مع مع ال classes حيث انه اذا لم يكن موجود في ال namespace لم يتم استدعاؤه من ال global namespace

<br>
<br>

### PSR-4
php standard recommendation
مبدأ ال psr-4 بيعتمد ان ال namespace اللي بضع فيه ال class الخاصه بي . بيمثل ال structure تبع المجلدات

لازم اتأكد ان ال class بيبدأ بحرف كابيتال واسم الفايل الخاص به يبدأ بحرف كابيتال واتأكد من حالة الاحرف في ال namespace لانها لو خطأ هيشتغل عادي في وندوز . ولكن عند رفع الشغل علي السيرفر اللي بيعمل ب lenux هيعطيني خطأ

### Autoload
لو دخلنا علي ال [composer.json]
هنلاقي الكود ده 
```json
"autoload": {
    "psr-4": {
        "App\\": "app/",
        "Database\\Factories\\": "database/factories/",
        "Database\\Seeders\\": "database/seeders/"
    }
},
```

احنا بقي لو عاوزين نعمل autoload من الاول
باستخدام ال php
```http
autoload.php
```
```php
function load_class($classname)
{
    include __DIR__ . "/{$classname}.php"
}
spl_autoload_register('load_class')
```
>لاحظ اننا داخل ال spl_autoload_register استخدمنا callback function
> يمكن ايضا استخدام anonymouse function أو closure function
>بهذا الشكل
```http
autoload.php
```
```php

spl_autoload_register(function($classname){
    include __DIR__ . "/{$classname}.php"
})
```
<br>
<br>

```http
/A/B/Person.php
```
```php
namespace A\B;

class Person
{
    const MALE = 'm';
    const FEMALE = 'f';
    public $name;
    protected $gender ;
    private $age ;
    public function __construct(){
        echo __class__ ;
    }
    public static setAge($age){
        $this->age = $age;
        return $this;
    }
    public static setGender($gender){
        $this->gendePr = $gender;
        return $this;
    }    
}
```
<br>
<br>

```http
/B/Person.php
```
```php
namespace B;

function hello(){
    echo 'hello already run !!';
}

class Person
{
    const MALE = 'm';
    const FEMALE = 'f';
    public $name;
    protected $gender ;
    private $age ;
    public function __construct(){
        echo __class__ ;
    }
    public static setAge($age){
        $this->age = $age;
        return $this;
    }
    public static setGender($gender){
        $this->gendePr = $gender;
        return $this;
    }    
}
```

<br>
<br>

```http
app.php
```
```php
namespace A;

include __DIR__ . '/autoload.php';



use \A\B\Person.php;
use \B\Person.php as PersonB;

hello(); // hello already run !!

$person = new Person ;
$person2 = new PersonB ;

$person->name = 'mohamed salama';


$person::$country = 'palastine';
$person2::$country = 'canada';

echo $person::$country; // palastine
echo $person2::$country; // canada
echo Person::$country;  // palastine

echo $person::MALE;  // m
```
> لاحظ ان ال hello function اشتغلت بالرغم من انها خارج ال class لكنها اشتغلت علشان استدعينا الفايل اللي بداخله ال class

> ال autoload بيتنفذ فقط علي مستوي
> 1. ال interfaces
> 2. ال classes
> 3. ال traits

### ال traits
ال trait هي مصطلح خاص بلغة ال php بنستخدمه داخل ال classes
لو لاحظنا ان جميع ال person class بتحتوي علي الميثود setAge و setGender
ممكن نمسحها منهم واستخدام ال trait بهذا الشكل

```http
Info.php
```
```php
trait Info 
{
    public static setAge($age){
        $this->age = $age;
        return $this;
    }
    public static setGender($gender){
        $this->gendePr = $gender;
        return $this;
    }    
}
```
> لاحظ ان ال trait مش class
> مينفعش استخدمها بهذا الشكل

```php
// new Info(); //wrong
```

> ال trait هي عبارة عن block انا بضع بداخله method
> ال trait لا اضع بداخله properties
> الهدف من ال trait هو استخدام ال methods اللي بداخله في أكثر من class بدون الحاجه الي عمل inhertance 
> ال trait بيكون افضل من ال inhertance لان ال class اللي احنا شغالين عليه ممكن يكون عامل inhertance ل class تاني
> لاحظ ان ال class لا يمكنه عمل سوي inhertance واحد فقط 
> لذلك احنا ممكن نعتبر ال trait نعتبره multible inheritance

<br>
<br>

```http
/A/B/Person.php
```
```php
namespace A\B;
use \Info;
class Person
{
    use Info;
    const MALE = 'm';
    const FEMALE = 'f';
    public $name;
    protected $gender ;
    private $age ;
    public function __construct(){
        echo __class__ ;
    }
}
```
<br>
<br>

```http
/B/Person.php
```
```php
namespace B;
use \Info;
function hello(){
    echo 'hello already run !!';
}

class Person
{
    use Info;
    const MALE = 'm';
    const FEMALE = 'f';
    public $name;
    protected $gender ;
    private $age ;
    public function __construct(){
        echo __class__ ;
    }
}
```
> لاحظ ان ال use اللي بالخارج دي الخاصة بال namespace 
> اللي داخل ال class بتعمل include لل trait داخل ال class

<br>
<br>

> لاحظ انه يمكننا اننا نعمل overwrite علي ال methods اللي موجوده في ال trait علي مستوي ال class الحالي

```http
/B/Person.php
```
```php
namespace B;
use \Info;
function hello(){
    echo 'hello already run !!';
}

class Person
{
    use Info;
    const MALE = 'm';
    const FEMALE = 'f';
    public $name;
    protected $gender ;
    private $age ;
    public function __construct(){
        echo __class__ ;
    }
    public static setAge($age){
        $this->age = $age + 1;
        return $this;
    }
}
```
#### استخدام callback function من داخل class 

ممكن نستدعيها 

```http
autoload.php
```
```php
class Autoloader
{
    public function register($classname)
    {
        include __DIR__ . "/{$classname}.php";
    }
}

$a = new Autoloader;
spl_autoload_register($a->register($classname));
```



<br>
<br>

وممكن نمررها

```http
autoload.php
```
```php
class Autoloader
{
    public function register($classname)
    {
        include __DIR__ . "/{$classname}.php";
    }
}
$a = new Autoloader;
spl_autoload_register([$a,'register']);
```

<br>
<br>

ولو static ممكن نمررها بدون ما نعرف ال class

```http
autoload.php
```
```php
class Autoloader
{
    public static function register($classname)
    {
        include __DIR__ . "/{$classname}.php";
    }
}

spl_autoload_register(['Autoloader','register']);
```
ممكن نمررها ك string كما بالاعلي
وممكن نمررها ك special constant كما بالاسفل 
وهذا افضل لتجنب كتابة قيمة ال namespace
```http
autoload.php
```
```php
class Autoloader
{
    public static function register($classname)
    {
        include __DIR__ . "/{$classname}.php";
    }
}

spl_autoload_register([Autoloader::class,'register']);
```

<br>
<br>


الطرق المتبعه في تمرير ال callback

1. function
    ```php
    spl_autoload_register('myFunction');
    ```
2. closure function
    ```php
    spl_autoload_register(function(){/*...*/});
    ```
3. method in class
    ```php
    spl_autoload_register([$varMyClass,'methodInClass']);
    ```

### inheritance php

```http
/B/Person.php
```
```php
namespace B;
use \Info;
use \A\B\Person as PersonA;

class Person extends  PersonA
{
    use Info;
    const MALE = 'm';
    const FEMALE = 'f';
    public $name;
    protected $gender ;
    private $age ;
    public function __construct(){
        echo __class__ ;
    }
    public static setAge($age){
        $this->age = $age + 1;
        return $this;
    }
}
```

لاحظ ان في هذا المثال PersonA هو أب ل Person

### Interfaces

لاحظ انه لا يمكن تعريف لل interface غير ال public

```http
/B/Human.php
```
```php
namespace B;
interface Human
{
    function setAge($age)
}
```

```http
/B/Person.php
```
```php
namespace B;
use \Info;

class Person implements Human
{
    use Info;
    const MALE = 'm';
    const FEMALE = 'f';
    public $name;
    protected $gender ;
    private $age ;
    public function __construct(){
        echo __class__ ;
    }
    public static setAge($age){
        $this->age = $age + 1;
        return $this;
    }
}
```
> لاحظ انني اذا حذفت ال use Info اللي بتحتوي علي setAge method سوف يعطيني error
> لاحظ ان ال interface تعتبر inhertance أو تعتبر اب لل person

<br>
<br>
علشان نتأكد من ان ال object  من class معين بنستخدم

```php
instanceof
```
<br>
<br>

```http
/A/Person.php
```
```php
namespace A;
use \B\Person.php as PersonB;
class Person extends PersonB
{
  /* ... */
}
```
<br>
<br>

```http
/B/Person.php
```
```php
namespace B;

class Person implements Human
{
 /* ... */
}
```
<br>
<br>

```http
app.php
```
```php
namespace A;

include __DIR__ . '/autoload.php';

use \A\Person.php as PersonA;
use \B\Person.php as PersonB;
use \B\Human.php;
$personA = new PersonA ;
$personB = new PersonB ;

if($personA instanceof PersonA){ /* true */ } // 1
if($PersonA instanceof PersonB){ /* true */ } // 2

if($personB instanceof PersonB){ /* true */ }  // 3
if($personB instanceof PersonA){ /* false */ }  // 4

if($personA instanceof Human){ /* false */ }  // 5
if($personB instanceof Human){ /* true */ }  // 6

```
> 1. هل الاوبجكت personA variable ابن من ال PersonA class ؟
 نعم
> 2. هل الابجكت personA variable ابن من ال PersonB class ؟
    نعم
> 3. هل الاوبجكت personB variable ابن من ال PersonB class ؟
 نعم
> 4. هل الابجكت personB variable ابن من ال PersonA class ؟
    لا . لان العكس هو الصحيح
> 5. هل الاوبجكت personA variable ابن من ال Human interface ؟
 نعم لان ال PersonA ابن من PersonB
> 6. هل الاوبجكت personB variable ابن من ال Human interface ؟
 نعم

### overloading php
مفيش عندنا overloading في ال  php

```http
/B/Person.php
```
```php
namespace B;

class Person
{
 public function setCountry($country){
    // ...
 }
 public function setCountry($country,$city){
    // ... errror
 }
}
```

البديل لل overloading في ال php
هو استخدام ال optional parameter

```http
/B/Person.php
```
```php
namespace B;

class Person
{
 public function setCountry($country,$city=''){
    // ...
 }
}
```

### self keyword
علشان نستدعي من نفس ال class بنستخدم ال self keyword

```http
/Parent.php
```
```php
class Parent
{
    $contry; 
}
```
<br>
<br>

```http
/Child.php
```
```php


class Child extends Parent
{
    $contry;
    public static function setCountry($country){
        self::$country= $country;
    }

    public function setCountry2($country){
        self::$country= $country;
    }
}
```
> لاحظ اننا ممكن نستخدم ال keyword اللي اسمها self مع الغير static

### parent keyword
علشان نستدعي ال الاب من ال الابن بنستخدم 
ال parent keyword

```http
/Parent.php
```
```php
class Parent
{
    $contry; 
}
```

<br>
<br>

```http
/Child.php
```
```php


class Child extends Parent
{
    $contry;
    public static function setCountry($country){
        parent::$country= $country;
    }

    public function setCountry2($country){
        parent::$country= $country;
    }
}
```
> لاحظ اننا ممكن نستخدم ال keyword اللي اسمها parent مع الغير static

### static keyword
علشان نستدعي الابن من الاب بنستخدم ال static keyword

ممكن ايضا ال static keyword يكون ليها وظائف اخري حيث انها تعمل كالتالي 
تبحث في ال self لو مش موجود هتبحث في الاب لو مش موجود هتبحث في الابن


```http
/Parent.php
```
```php
class Parent
{
    public static function setCountry($country){
        static::$country= $country;
    }

    public function setCountry2($country){
        static::$country= $country;
    }
}
```

<br>
<br>

```http
/Child.php
```
```php
class Child extends Parent
{
    $contry;
}
```
> * ال self keyword بيعمل ديما referance لنفسه
> * ال parent keyword بيعمل ديما referance للاب
> * ال static keyword بيعمل referance
>   1. لنفسه . لو مش موجود
>   2. للاب .  لو مش موجود
>   3. للاب . (وفي الغالب بيستخدم علشان يعمل referance للابن)

> لاحظ ان ال self keyword, parent keyword, static keyword  ممكن نستخدمهم في ال static method وفي الغير static 

> لاحظ ان ال self تعمل عمل ال this

> لاحظ ان ال static method لا يمكن ان تستخدم ال this ولكن تستخدم ال self بدلا عنها

</div>




### Magic Method

#### CallStatic Magic Method

```php

class MyClass
{
    public static function __callStatic($name, $arguments)
    {
        return  $name;
    }
}
print_r(MyClass::MyTest()); // MyTest
```
<br>

```php
class MyClass
{
    public static function __callStatic($name, $arguments)
    {
        return  $arguments;
    }
}
print_r(MyClass::MyTest('a','b','c')); // Array ( [0] => a [1] => b [2] => c )
```

#### Set and Get Magic Method

```php
class MyClass
{
    public $x = [];
    public function __set($name, $value)
    {
        return  $this->x[$name]=$value;
    }

    public function __get($name)
    {
        return  isset($this->x[$name])?$this->x[$name]:'not found';
    }
}
$a = new MyClass;
print_r($a->aaa); // not found
$a->bbb = 'Hi';
print_r($a->bbb); // Hi
```

#### ToString Magic Method

```php
class MyClass
{
    public function __toString()
    {
        return 'Hi';
    }
}
$a = new MyClass;
print_r($a); // MyClass Object ( )
echo $a ; // Hi
```

### facade and service container


```http
/Person.php
```
```php
class Person
{
    public static $country ;
    public $name = 'mohamed samir';
    public $age = 20;

    public function __construct()
    {
        //echo __class__ ;
    }
    public static function setCountry($country)
    {
        self::$country = $country ;
    }
    public function myName(){
        return $this->name ;
    }
    public function myAge(){
        return $this->age ;
    }
}
```

<br>
<br>

```http
/ServiceContainer.php
```
```php
class ServiceContainer
{
    protected static $container=[];
    public static function bind($name,$instance)
    {
        self::$container[$name] = $instance;
    }

    public static function make($name)
    {
        return  self::$container[$name];
    }
}
```


<br>
<br>

```http
/PersonFacade.php
```
```php
class PersonFacade{
    protected static $container = 'person';
    public static function __callStatic($name, $arguments=[])
    {
        $person = ServiceContainer::make(self::$container);
        return $person->$name(...$arguments);
    }
}
```




<br>
<br>

```http
/index.php
```
```php
include './Person.php';
include './PersonFacade.php';
include './ServiceContainer.php';

$person = new Person ;

// echo ($person->myName());
ServiceContainer::bind('person',$person);
// echo (ServiceContainer::make('person')->myName());
echo PersonFacade::myName();
```

