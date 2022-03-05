# 1. Для чего используется метод Function.prototype.apply и call?
```
Apply используется для привязки определенного объекта к значению this вызываемой функции.
Этот метод похож на Function.prototype.call. Единственное отличие состоит в том, что в apply аргументы передаются в виде массива.

const person = {
    name: 'Marko Polo'
}

function greeting(greetingMessage) {
    return `${greetingMessage} ${this.name}`
}

greeting.apply(person, ['Hello']) // Hello Marko Polo

```
# 2. Что такое область видимости (Scope)?
```
Область видимости — это место, где (или откуда) мы имеем доступ к переменным или функциям. JS имеем три типа областей видимости: глобальная, функциональная и блочная (ES6).

Глобальная область видимости — переменные и функции, объявленные в глобальном пространстве имен, имеют глобальную область видимости и доступны из любого места в коде.

var g = 'global'

function globalFunc() {
    function innerFunc() {
        console.log(g) // имеет доступ к переменной g, поскольку она является глобальной
    }
    innerFunc()
}

Функциональная область видимости (область видимости функции) — переменные, функции и параметры, объявленные внутри функции, доступны только внутри этой функции.

function myFavouriteFunc(a) {
    if (true) {
        var b = 'Hello ' + a
    }
    return b
}
myFavouriteFunc('World')

console.log(a) // Uncaught ReferenceError: a is not defined
console.log(b) // не выполнится

Блочная область видимости — переменные (объявленные с помощью ключевых слов «let» и «const») внутри блока ({ }), доступны только внутри него.

function testBlock() {
    if (true) {
        let z = 5
    }
    return z
}

testBlock() // Uncaught ReferenceError: z is not defined
```

# 3. Какое значение имеет this?
```
Обычно this ссылается на значение объекта, который в данный момент выполняет или вызывает функцию. «В данный момент» означает, что значение this меняется в зависимости от контекста выполнения, от того места, где мы используем this.

const carDetails = {
    name: 'Ford Mustang',
    yearBought: 2005,
    getName() {
        return this.name
    }
    isRegistered: true
}

console.log(carDetails.getName()) // Ford Mustang

В данном случае метод getName возвращает this.name, а this ссылается на carDetails, объект, в котором выполняется getName, который является ее «владельцем».

Добавим после console.log три строчки:

var name = 'Ford Ranger'
var getCarName = carDetails.getName

console.log(getCarName()) // Ford Ranger

Второй console.log выдает Ford Ranger, и это странно. Причина такого поведения заключается в том, что «владельцем» getCarName является объект window. Переменные, объявленные с помощью ключевого слова «var» в глобальной области видимости, записываются в свойства объекта window. this в глобальной области видимости ссылается на объект window (если речь не идет о строгом режиме).
```