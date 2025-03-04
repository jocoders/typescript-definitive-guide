## Важно
________________

Помимо того, что система типов *TypeScript* включает в себя все существующие в *JavaScript* типы данных, некоторые из них подверглись более очевидному уточнению.

Типы данных, чьи идентификаторы начинаются с прописной буквы (`Type`) представляют *объектные типы* (ссылочные типы), описывающие одноименные типы из *JavaScript* (`Number`, `String`, `Boolean` и т.д.). В *TypeScript* такие типы описаны с помощью глобальных интерфейсов (`interface`) и помимо того, что их можно указывать в аннотации типа, их также можно расширять (`extends`) и реализовывать (`implements`) другими объектными типами.  

Типы данных, чьи идентификаторы начинаются со строчной буквы (`type`) представляют литералы примитивных значений. Подобные типы определены ключевыми зарезервированными словами, что в свою очередь означает невозможность объявлять типы данных с идентичными идентификаторами. Типы данных, которые обозначают примитивы, можно указывать в аннотации, но их нельзя расширять или реализовывать объектными типами данных.


## Number (number) примитивный числовой тип
________________

В *TypeScript*, собственно как и в *JavaScript*,  все числа являются 64-битными числами двойной точности с плавающей точкой. 

Помимо того, что в *TypeScript* существует объектный тип `Number`, описывающий с помощью глобального интерфейса одноименный тип из *JavaScript*, также существует тип данных, который представляет примитивные значения числовых литералов, и который указывается с помощью ключевого слова `number`. Кроме того, тип `number` неявно преобразуется в тип описанный с помощью глобального интерфейса `Number`, что делает его совместимым с ним, но не наоборот.

~~~~~typescript
let v1: number; // v1: number explicit
let v2 = 5.6; // v2: number implicit
~~~~~

Напомню, что числа могут записываться в двоичной, восьмеричной, десятичной, шестнадцатеричной системе счисления. 

~~~~~typescript
let binary: number = 0b101;
let octal: number = 0o5;
let decimal: number = 5;
let hex: number = 0x5;
~~~~~

В *TypeScript*, поведение типа `Number` идентично поведению одноименного типа в *JavaScript*.


## String (string) примитивный строковой тип
________________

Примитивный тип `String` представляет собой последовательность символов в кодировке *Unicode* *UTF-16*. Строки должны быть заключены в одинарные, двойные или обратные кавычки, последний из которых являются инициаторами, так называемых шаблонных строк.

Помимо того, что в *TypeScript* существует объектный тип `String`, описывающий одноименный тип из *JavaScript*, также существует тип данных, который представляет примитивные значения строковых литералов, и который указывается с помощью ключевого слова `string`. Кроме того, тип `string` неявно преобразуется в тип описанный с помощью глобального интерфейса `String`, что делает его совместимым с ним, но не наоборот.

~~~~~typescript
let v1: string; // v1: string explicit
let v2 = 'Bird'; // v2: string implicit
let v3: string = "Fish"; // v3: string explicit
let v4: string = `Animals: ${ v2 }, ${ v3 }.`; // v4: string explicit
~~~~~

В *TypeScript*, поведение типа `String` идентично поведению одноименного типа в `JavaScript`.


## Boolean (boolean) примитивный логический тип
________________

Примитивный тип `Boolean` является логическим типом и представляется значениями (истина) `true` либо (ложью) `false`. 

Помимо того, что в *TypeScript* существует объектный тип `Boolean`, описывающий одноименный тип из *JavaScript*, также существует тип данных, который представляет примитивные значения логических литералов, и который указывается с помощью ключевого слова `boolean`. Кроме того, тип `boolean` неявно преобразуется в тип описанный с помощью глобального интерфейса `Boolean`, что делает его совместимым с ним, но не наоборот.

~~~~~typescript
let isV1Valid: boolean; // explicit
let isV2Valid = false; // implicit
~~~~~

В *TypeScript*, поведение типа `Boolean` идентично поведению одноименного типа в *JavaScript*.


## Symbol (symbol) примитивный символьный тип
________________

Примитивный тип `Symbol` предоставляет уникальные идентификаторы, которые при желании могут использоваться в качестве индексируемых членов объекта. 

Помимо того, что в *TypeScript* существует объектный тип `Symbol`, описывающий одноименный тип из *JavaScript*, также существует тип данных, который представляет примитивные значения числовых литералов, и который указывается с помощью ключевого слова `symbol`. Кроме того, тип `symbol` неявно преобразуется в тип описанный с помощью глобального интерфейса `Symbol`, что делает его совместимым с ним, но не наоборот.

~~~~~typescript
let v1: symbol; // v1: symbol explicit
let v2 = Symbol( 'animal' ); // v2: symbol implicit
~~~~~

Если указать `Symbol` в качестве ключа для члена класса, то в дальнейшем к нему можно будет обратиться только при наличии того же ключа. Но не будет лишнем напомнить, что при желании можно достать символ из самого объекта.

~~~~~typescript
class ZooTool {
  public readonly static CAGE_KEY: string = 'technical key';
}

class AnimalCage {
  [Symbol.for(ZooTool.CAGE_KEY)]() {
      console.log('open: ...');
  }
}

class KeeperZoo {
  key: symbol = Symbol.for(ZooTool.CAGE_KEY);
}

let lionCage: AnimalCage = new AnimalCage();
let keeperZoo: KeeperZoo = new KeeperZoo();

lionCage[keeperZoo.key](); // open: ...
~~~~~

Тип `symbol` предназначен для аннотирования символьных литералов. В *TypeScript*, поведение типа `Symbol` идентично поведению одноименного типа в `JavaScript`.



## BigInt (bigint) примитивный числовой тип
________________

`BigInt` примитивный числовой тип данных позволяющий работать с числами произвольной точности, позволяющий безопасно работать со значениями выходящими за пределы установленные типом `Number`. Примитивный тип `BigInt` указывается с помощью ключевого слова `bigint`.

~~~~~typescript
let bigInt: bigint = BigInt( Number.MAX_VALUE ) + BigInt( Number.MAX_VALUE );
~~~~~

Но стоит заметить, что на данный момент (конец 2018 года), из-за плохой поддержки типа `BigInt`, `TypeScript` позволяет работать с ним лишь при установленной опции компилятора в значение `esnext`.

Тип `bigint` предназначен для аннотирования числовых значений с произвольной точность. В *TypeScript*, поведение типа `BigInt` идентично поведению одноименного типа в `JavaScript`.


## Итог

Подведем итоги -

- В аннотации примитивов указываются типы, ключевые слова которых начинаются со строчной буквы - `number`, `string`, `boolean`, `symbol`.
- В случаях, когда в аннотации нуждаются ссылки на класс типа, указываются типы, чьи имена начинаются с прописной буквы.
