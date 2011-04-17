## Объекты и их свойства

В JavaScript всё является объектом, лишь за двумя исключениями — [`null`](#core.undefined) и [`undefined`](#core.undefined).

    false.toString() // 'false'
    [1, 2, 3].toString(); // '1,2,3'

    function Foo(){}
    Foo.bar = 1;
    Foo.bar; // 1

Неверно считать, что числовые литералы нельзя использовать в качестве объектов — это распространённое заблуждение. Его причиной является упущение в парсере JavaScript, благодаря которому применение *точечной нотации* к числу воспринимается им как литерал числа с плавающей точкой.

    2.toString(); // вызывает SyntaxError

Есть несколько способов обойти этот недостаток и любой из них можно использовать для того, чтобы работать с числами, как с объектами:

    2..toString(); // вторая точка распознаётся корректно
    2 .toString(); // обратите внимание на пробел перед точкой
    (2).toString(); // двойка вычисляется заранее

### Объекты как тип данных

Объекты в JavaScript могут использоваться как [*хеш-таблицы*][1]: подавляющей частью состоят из именованных свойств (ключей), привязанных к значениям.

Используя объектный литерал — нотацию `{}` — можно создать простой объект. Новый объект [наследуется](#object.prototype) от `Object.prototype` и не имеет [собственных свойств](#object.hasownproperty).

    var foo = {}; // новый пустой объект

    // новый объект со свойством 'test', имеющим значение 12
    var bar = {test: 12};

### Доступ к свойствам

Получить доступ к свойствам объекта можно двумя способами: используя либо точечную нотацию, либо запись квадратными скобками.

    var foo = {name: 'Kitten'}
    foo.name; // kitten
    foo['name']; // kitten

    var get = 'name';
    foo[get]; // kitten

    foo.1234; // SyntaxError
    foo['1234']; // работает

Обе нотации идентичны по принципу работы — одна лишь разница в том, что использование квадратных скобок позволяет устанавливать свойства динамически и использовать такие имена свойств, какие в других случаях могли бы привести к синтаксической ошибке.

### Удаление свойств

Единственный способ удалить свойство у объекта — использовать оператор `delete`; устанавливая свойство в `undefined` или `null`, вы только заменяете связанное с ним *значение*, но не удаляете *ключ*.

> **Замечание** от перев.: Если ссылок на значение больше нет, то сборщиком мусора удаляется и само значение, но ключ объекта при этом всё также имеет новое значение.

    var obj = {
        bar: 1,
        foo: 2,
        baz: 3
    };
    obj.bar = undefined;
    obj.foo = null;
    delete obj.baz;

    for(var i in obj) {
        if (obj.hasOwnProperty(i)) {
            console.log(i, '' + obj[i]);
        }
    }

Приведённый код выведет две строки: `bar undefined` и `foo null` — на самом деле удалено было только свойство `baz` и посему только оно будет отсутствовать в выводе.

### Запись ключей

    var test = {
        'case': 'Я — ключевое слово, поэтому меня надо записывать строкой',
        delete: 'Я тоже ключевое слово, так что я' // бросаю SyntaxError
    };

Свойства объектов могут записываться как явно символами, так и в виде закавыченных строк. В связи с другим упущением в парсере JavaScript, этот код выбросит `SyntaxError` во всех версиях ранее ECMAScript 5.

Источником ошибки является факт, что `delete` — это *ключевое слово* и поэтому его необходимо записывать как *строчный литерал*: ради уверенности в том, что оно будет корректно опознано более старыми движками JavaScript.

*От перев.:* И еще один пример в пользу строковой нотации, это относится к [JSON][2]:

    // валидный JavaScript и валидный JSON
    {
        'foo': 'oof',
        'bar': 'rab'
    }

    // валидный JavaScript и НЕ валидный JSON
    {
        foo: 'oof',
        bar: 'rab'
    }

[1]: http://ru.wikipedia.org/wiki/%D0%A5%D0%B5%D1%88-%D1%82%D0%B0%D0%B1%D0%BB%D0%B8%D1%86%D0%B0
[2]: http://ru.wikipedia.org/wiki/JSON
