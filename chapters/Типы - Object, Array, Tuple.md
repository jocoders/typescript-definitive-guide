## Типы - Object, Array, Tuple
________________

Пришло время рассмотреть такие типы данных как `Object` и `Array`, с которыми разработчики *JavaScript* уже хорошо знакомы. А также неизвестный в мире *JavaScript* тип данных `Tuple`, который не представляет из себя ничего сложного.


## Object (object) ссылочный объектный тип
________________

Ссылочный тип данных `Object` является базовым для всех ссылочных типов в *TypeScript*.

Помимо того, что в *TypeScript* существует объектный тип `Object`, описывающий с помощью глобального интерфейса одноименный тип из *JavaScript*, также существует тип данных, который представляет любое объектное значение, и который указывается с помощью ключевого слова `object`. Поведение типа указанного с помощью ключевого слова и интерфейса различаются.

Переменным, которым указан тип с помощью ключевого слова `object`, не могут хранить значения примитивных типов, чьи имена начинаются со строчной буквы. В отличии от них, тип интерфейс `Object`, совместим с любым типом данных.

~~~~~typescript
let elephant: object;
let lion: Object;


elephant = 5; // Error
lion = 5; // Ok

elephant = ""; // Error
lion = ""; // Ok

elephant = true; // Error
lion = true; // Ok

elephant = null; // Ok, strictNullChecks = false
lion = null; // Ok, strictNullChecks = false

elephant = undefined; // Ok, strictNullChecks = false
lion = undefined; // Ok, strictNullChecks = false
~~~~~

Ограничение объектов типом, указанным с помощью ключевого слова `object`, будет ограничивать их функционалом интерфейса `Object`. При попытке обратиться к членам объекта, не задекларированным в интерфейсе `Object`, возникнет ошибка. Напомним, что в случаях, когда тип нужно сократить до базового, сохранив при этом возможность обращения к специфичным (определенным пользователем) членам объекта, нужно использовать тип `any`.

~~~~~typescript
class SeaLion {
  rotate():void {}
  voice(): void {}
}

let seaLionAsObject: object = new SeaLion(); // Ok
seaLionAsObject.voice(); // Error

let seaLionAsAny: any = new SeaLion(); // Ok
seaLionAsAny.voice(); // Ok
~~~~~

Тип интерфейс `Object` идентичен по своей работе с одноименным типом из *JavaScript*. Несмотря на то, что тип ,указанный с помощью ключевого слова `object`, имеет схожее название, его поведение отличается от типа интерфейса.


## Array (type[]) ссылочный массивоподобный тип
________________

Ссылочный тип данных `Array` является типизированным спископодобным объектом, содержащим логику для работы с элементами.

Тип данных `Array` указывается с помощью литерала массива, перед которым указывается тип данных `type[ ]`.

Если  при объявлении массива указать тип `string[ ]`, то храниться в нем могут только элементы, принадлежащие или совместимые с типом `String` (например `Null`, `Undefined`, `Literal Type String`)

~~~~~typescript
var animalAll: string[] = [
 "Elephant",
 "Rhino",
 "Gorilla"
];

animalAll.push( 5 ); // Error
animalAll.push( boolean ); // Error
animalAll.push( null ); // Ok
animalAll.push( undefined ); // Ok
~~~~~

В случае неявного указания типа, вывод типов самостоятельно укажет тип как `string[ ]`.

~~~~~typescript
var animalAll = [
 "Elephant",
 "Rhino",
 "Gorilla"
]; // animalAll : string[ ]
~~~~~

Если массив должен хранить смешанные типы данных, то один из способов указать тип объединение (`Union`). Нужно обратить внимание, на то, как трактуется тип данных Union при указании его массиву. Может показаться, что указав в качестве типа тип объединение Union, массив может состоять только из какого-то одного перечисленного типа `Elephant`, `Rhino` или `Gorilla`. Но это не совсем так. Правильная трактовка гласит, что каждый элемент массива может принадлежать к типу `Elephant` или `Rhino` или `Gorilla`.

~~~~~typescript
class Elephant {}
class Rhino {}
class Gorilla {}


var animalAll: ( Elephant | Rhino | Gorilla )[] = [
new Elephant(),
new Rhino(),
new Gorilla()
];
~~~~~

Если для смешанного массива не указать тип явно, то вывод типов самостоятельно укажет все типы, которые хранятся в массиве. Более подробно эта тема будет рассмотрена в главе [“Типизация - Вывод типов”]().

В случае, если типы данных неизвестны  при создании экземпляра массива, следует указать в качестве типа тип `any`.

~~~~~typescript
let dataAll: any[] = [];

dataAll.push(5); // Ok -> number
dataAll.push('5'); // Ok -> string
dataAll.push(true); // Ok -> boolean
~~~~~

Нужно стараться использовать, как можно реже массивы со смешанными типами, а к массивам с типом `any` нужно прибегать только в самых крайних случаях. Кроме того, как было рассказано в главе [“Экскурс в типизацию - Совместимость типов на основе вариантности”](), из-за того, что нужно крайне осторожно относится к массивам, у которых входные типы являются ковариантными, не рекомендуется указывать массиву базовые типы. 

В случаях, в которых требуется создавать массив при помощи оператора new, приходится прибегать к типу глобального обобщённого интерфейса `Array<T>`. Обобщения будут рассмотрены чуть позднее, а пока нужно запомнить следующее.

При попытке создать экземпляр массива путем вызова конструктора, операция завершится успехом в тех случаях, когда создаваемый массив будет инициализирован пустым, либо с элементами одного типа данных. В случаях смешанного массива, необходимо явно указывать типы обобщения, заключенные в угловые  скобки.

~~~~~typescript
let animalData: string[] = new Array(); //Ok
let elephantData: string[] = new Array("Dambo"); // Ok
let lionData: (string | number)[];

lionData = new Array("Simba", 1); // Error
lionData = new Array("Simba"); // Ok
lionData = new Array(1); // Ok
let deerData: (string | number)[] = new Array< string | number >("Bambi", 1); // Ok
~~~~~

В *TypeScript*, поведение типа `Array` идентично поведению одноименного типа в *JavaScript*.


## Tuple ([ T0, T1, …, Tn ]) тип кортеж
________________

Тип данных `Tuple` (кортеж) состоит из строго заданной последовательности типов, череда которых соответствует индексам, указанного в качестве значения массива, начиная с нуля. Тип данных Tuple указывается литералом массива, в котором заключены, перечисленные через запятую, типы данных `[ T1, T2, T3 ]`.

Массив, выступающий в качестве значения, должен хранить элементы таким образом, чтобы в индексах, соответствующих расположению типов в кортеже, обязательно находились значения совместимые с этими типами.

Другими словами, если переменной, аннотированной типом кортеж имеющим определенную последовательность, состоящую из типов `string` и `number`, присвоить ссылку на массив у которого первым элементом хранится строка, а вторым цифра, операция присваивания завершится успехом. Если в массиве первым элементом будет значение `null`, а вторым `undefined`, то ошибка выброшена не будет, так как эти типы считаются  совместимыми с указанными в кортеже. Но если первым элементом будет цифра, а вторым строка, будет выброшена ошибка, так как не соблюдена заданная последовательность типов.

~~~~~typescript
let elephantData: [ string, number ] = [ 'Dambo', 1  ]; // Ok
let giraffeData: [ string, number ] = [ null, undefined ]; // Ok
let liontData: [ string, number ] = [ 1, 'Simba'  ]; // Error
~~~~~

Кроме того, длина массива выступающего в качестве значения, должны соответствовать количеству типов указанных в `Tuple`.

~~~~~typescript
let elephantData: [ string, number ] = [ 'Dambo', 1  ]; // Ok
let liontData: [ string, number ] = [ 'Simba', 1, 1  ]; // Error, лишний элемент
let fawnData: [string, number] = ['Bambi']; // Error, не достает одного элемент
let giraffeData: [ string, number ] = [  ]; // Error, не достает всех элементов
~~~~~

Но это правило не мешает добавить новые элементы после того,как массив был присвоен ссылке (ассоциирован с ссылкой). Но элементы, чьи индексы выходят за пределы установленные кортежем, обязаны иметь тип совместимый с одним из перечисленных в этом кортеже.

~~~~~typescript
let elephantData: [ string, number ] = [ 'Dambo', 1  ];
elephantData.push( 1941 ); // Ok
elephantData.push('Disney'); // Ok
elephantData.push( true ); // Error, тип boolean, в то время, как допустимы только типы совместимые с типами string и number

elephantData[ 10 ] = ''; // Ok
elephantData[ 11 ] = 0; // Ok

elephantData[ 0 ] = ''; // Ok, значение совместимо с типом заданном в кортеже
elephantData[ 0 ] = 0; // Error, значение не совместимо с типом заданном в кортеже
~~~~~

Массив, который связан с типом кортежем, ничем не отличается от обычного, за исключение того, как определяются типы его элементов.


При попытке присвоить элемент под индексом 0 переменной с типом `string`, а элемент под индексом `1` переменной с типом `number`, операции присваивания завершатся успехом. Но несмотря на то, что элемент под индексом `2` принадлежит к литеральному типу `string`, при попытке присвоить его в качестве значения переменной с типом `string`, будет выброшена ошибка. Дело в том, что элементы, чьи индексы выходят за пределы установленные кортежем, принадлежат к типу объединению (`Union`). Это означает что элемент  под индексом `2` принадлежит к типу `string | number`, а это не тоже самое что тип `string`, который указан переменной.

~~~~~typescript
let elephantData: [ string, number ] = [ 'Dambo', 1  ]; // Ok

elephantData[ 2 ] = 'nuts';

let elephantName: string = elephantData[ 0 ]; // Ok, type string
let elephantAge: number = elephantData[ 1 ]; // Ok, type number
let elephantDiet: string = elephantData[ 2 ]; // Error, type string | number
~~~~~

Есть два варианта решения этой проблемы. Первый вариант, это изменить тип переменной со `string` на тип объединение `string | number`, что ненадолго избавит от проблемы совместимости типов. Второй вариант, более подходящий вариант, прибегнуть к приведению типов, который детально будет рассмотрен позднее.

В случае, если описание кортежа может навредить семантике кода, его можно поместить в описание псевдонима типа (`type`).

~~~~~typescript
type Tuple = [number, string, boolean, number, string];

let v1: [number, string, boolean, number, string]; // Bad
let v2: Tuple; // Good
~~~~~

Кроме того, тип кортеж можно указывать в аннотации остаточных параметров (`...rest`).

~~~~~typescript
function f( ...rest: [ number, string ,boolean ] ): void {}

let tuple: [ number, string, boolean ] = [ 5, '', true ];
let array = [ 5, '', true ];

f( 5 ); // Error
f( 5, '' ); // Error
f( 5, '', true ); // Ok
f( ...tuple ); // Ok
f( tuple[0], tuple[1], tuple[2] ); // Ok
f(...array); // Error
f( array[0], array[1], array[2] ); // Error, все элементы массива принадлежат к типу string | number | boolean, в то время как первый элемент кортежа принадлежит к типу number
~~~~~

Помимо этого, типы указанные в кортеже могут быть помечены как необязательные с помощью необязательного модификатора  `?`.

~~~~~typescript
function f( ...rest: [ number, string?, boolean? ] ): void {}

f( ); // Error
f( 5 ); // Ok
f( 5, '' ); // Ok
f( 5, '', true ); // Ok
~~~~~

У кортежа, который включает типы помеченные как не обязательные, свойство длины принадлежит к  типу объединению состоящего из литеральных числовых типов.

~~~~~typescript
function f( ...rest: [ number, string?, boolean? ] ): [ number, string?, boolean? ] {
  return rest;
}

let l = f( 5 ).length; // let l: 1 | 2 | 3
~~~~~

Еще несколько неочевидных моментов связанным с выводом типов при работе с типом кортеж будет рассмотрено в главе [“Типизация - Вывод типов”](). 

И последнее на что стоит обратить внимание, что кортеж не указывается выводом типов при не явном указании.

~~~~~typescript
let elephantData = [ 'Dambo', 1 ]; // type Array (string | number)[]
~~~~~


Тип `Tuple` является уникальным для `TypeScript`, в `JavaScript` подобного типа не существует.


## Итоги

Подведем итоги -


- Поведение типа глобального интерфейса `Object` отличается от типа указанного с помощью ключевого слова `object`.
- Объект с типом `object` является базовым типом для всех ссылочных типов и ограничен *api* экземпляра класса `Object`.
- Ссылочный тип `Array` обозначается с помощью литерала массива перед которым обязательно указывается тип `type[ ]`.
- Очень важным моментом является понимание того, что указывается не тип данных, из которого состоит массив, а указывается тип данных к которому может принадлежать каждый элемент массива.
- Тип данных `Tuple` (кортеж) состоит из строго заданной последовательности типов, череда которых соответствует индексам, указанного в качестве значения массива, начиная с нуля и обозначается литералом массива в котором заключены перечисленные типы данных.
- Элементы, чьи индексы выходят за предел установленным кортежем, должны быть совместимы с одним из перечисленных в кортеже типов.
- Элементы, чьи индексы выходят за границы указанные кортежем, при присвоении имеют тип объединения (`Union`), а также не могут быть получены путем деструктуризации.
- Кортеж не выводится с помощью вывода типов.
