---
layout: default
title: Курс Android разработки в Технополисе 2022
---


# Лекция №1: Kotlin

[Презентация](./kotlin_presentation.pdf)

Kotlin – это статически типизированный, объектно-ориентированный, современный, кросс-платформенный язык
программирования. Разработка началась в 2010 году и ведется до сих пор.

Котлин полностью совместим с Java кодом, это его главное преимущество,
то есть он компилируется в тот же байт код что и Java и работает на той же jvm что и Java.
Поэтому вы можете использовать котлин в уже существущем проекте без каких либо проблем.
Котлин с 2019 года является приоритетным языком программирования под Андроид.

## Почему появился?

Перед JetBrains встала задача стать более продуктивными с полной совместимостью старых разработок. В первую очередь с
Java, а затем уже с JS, C++.

## Отличия от Java
1. Лаконичность
2. Null safety
3. Smart cast
4. Global functions
5. Extensions
6. Lambdas
7. Нет проверяемых exceptions

## Null safety
Поговорим о самом важном отличии от Java – это нулл сейфти
В java есть один exception - NullPointerException который выбрасиывается когда мы вызываем методы у нулевого объекта. 
Джава нам не гарантирует, что переменная не является null. В котлине эта проблема решается более строгой типизацией: nullable и notnull
Пример
```kotlin
var s: String = "i'm string"
s = null // ошибка

var optString: String? = "foo"
optString = null
```
Поэтому если мы знаем, что объект может быть null то приписываем ? к типу
Чтобы вызвать методы у nullable типа нужно добавлять знак вопроса ?
```kotlin
var s: String? = "some string"
val size = s?.length ?: 0
if (s != null) {
    val firstChat = s.first() // 's'
}
```
Переменной size присвоится значение длины строки или 0, в этом нам поможет оператор elvis.
Далее нам приходит на помощь idea с возможностью smartcast 
в проверке if (s != null) среда разработки понимает что s уже не просто опшнл String,
а ненулевая строка поэтому подсечивает s на след строчке зеленым цветом. 
Это очень удобно при написании кода, вы всегда знаете с каким типом имеете дело.


## Синтаксис языка
А теперь перейдем к синтаксису языка.

Разработчики языка использовали элементы и заимствовали опыт многих языков программирования, например, 
таких, как Java, Scala, C#, Groovy, C++, JavaScript.

### Переменные
Чтобы объявить переменную требуется ключевое слово `val` или `var`.

```kotlin
var a = 4
a = 8
println("a = $a") // a = 2

val b = 15
b = 16 // ошибка!
```
`var` означает что переменная может быть изменена, а `val` что переменная является неизменяемой
То есть в приведенном выше примере код не скомпилируется, тк переменной `b` присваивается новое значение

При объявлении полей класса иногда нужно указывать тип переменной. 
Делается это следующим образом:
```kotlin
class File {
    var name: String = ""
}
```

### Функции
Следующая важная часть языка это функции.

Функция объявляется с помощью ключевого слова `fun`.

```kotlin
fun helloWorld() {
    println("Hello, world!")
}
```
Данная функция выводит Hello world в консоль.

У функции могут быть аргументы и возвращаемый тип
```kotlin
fun add(array: IntArray, element: Int): IntArray {
    return array + element
}
```
В данном случае мы добавляем к массиву array новый элемент.
Что мы здесь видим из интересного? функция `+` на самом деле является оператором. Такое вы могли видеть в c++
Котлин позволяет вам использовать заранее объявленные операторы так и объявлять свои
в данном случае + это оператор `plus`


Аргументы могут иметь значение по умолчанию
```kotlin
fun updateColor(color: Color = Color.RED) {
    this.color = color
}
```

При вызове функции с аргументами мы можем явно указывать имя такого параметра.

```kotlin
fun drawView() {
    updateColor(color = Color.BLACK)
}
```

Одна из отличительных особенной котлин это функции расширения. Они улучшают читаемость кода, 
есть даже архитектурный подход extensions oriented programming. 
Например:


```kotlin
fun Int.isZero(): Boolean = this == 0

fun MutableList<Int>.swap(index1: Int, index2: Int) {
    val tmp = this[index1] // 'this' corresponds to the list
    this[index1] = this[index2]
    this[index2] = tmp
}
```

## Классы

### open/final

Работа с классами похожа на то как это происходит в Java, так же нет множественного наследования, присутствуют
абстрактные классы, интерфейсы, енумы. 
Но когда мы объявляем класс в джава от него можно унаследоваться по умолчанию, в
котлин же все классы по умолчанию final. 
Если мы хотим сделать класс наследуемым, то следует указать ключевое слово `open`.

```kotlin
open class MyClass {

}

class MySuperClass : MyClass() {

}

class MySecondSuperClass : MySuperClass() // ошибка! MySuperClass является final
```

### Any/java.lang.Object

По аналогии с Java суперклассом для всех объектов в Kotlin по умолчанию является kotlin.Any (как java.lang.Object)

### Конструкторы

Можно создать несколько конструкторов, в которых можно задать дефолтные значения каким-то пропертям.

```kotlin
class Student(
    val firstName: String = "John",
    val lastName: String = "Doe"
) {

    constructor(map: Map<String, String>) : this(
        firstName = map.getOrDefault("firstName", "John"),
        lastName = map.getOrDefault("lastName" , "Doe")
    )

    constructor(string: String) : this(map = string.toMap())
    
    init {
        println("created")
        // other logic
    }
}
```
Данный класс имеет праймари конструктор который объявлен рядом с названием класса и 2 вторичных конструктора
Иногда нам требуется создать объект разными способами, вторичные конструкторы приходят нам на помощь

Что мы здесь видим? мы видим что объявление property класса (аналог полей из Java) происходит в праймари конструкторе
Эти проперти неизменямые и имеют дефолтные значения


### Properties

```kotlin
class Cat {
    var name: String = "Семён"
        set(value) {
            println(value)
            if (value.isEmpty()) return
            field = value
        }

    val age: Int
        get() = 10

    val lifeMoments: List<Moment> by lazy {
        longAndHeavyCalculationsOfLifeMoments()
    }

    lateinit var owner: Human
}
```

В отличие от Java в котлин классе используются проперти(свойства), к которым можно переопределить getter и setter

Свойства могут быть изменяемые (`var`) и неиз"меняемые (`val`). 
Так же можно их объявить как `lateinit var`. 
Это полезно, когда мы 100% получим значение чуть позже и сможем им воспользоваться.


### Visibility modifiers

Проперти класса могут иметь доступы public, private, internal Но отсутствует такое видимость package-private!
- Public используется по-умолчанию, код будет виден отовсюду;
- Private: будут видны только внутри класса/файла;
- Protected: видны наследникам класса и внутри файла;
- Internal: видно повсюду в рамках одного gradle-модуля;
- В отличие от java в kotlin нет package-only-visible модификатора.

### Синглтон

Класс можно объявить как синглтон при помощи ключевого слова object

```kotlin
object Logger {
    fun log() {
        
    }
}
```

Нельзя создать экземпляр данного класса Logger, он может быть только один.

### Sealed class/interface

В котлин есть чудесная фича под названием sealed class/interface
when/if expression

```kotlin
sealed class State {
    object Loading : State() {
        const val message = "Loading"
    }
    class Loaded(val items: List<String>): State()
    class Failure(val throwable: Throwable): State()
}

```

При получении объекта типа State мы можем понять какой конкретно подкласс имеется в виду, среда разработки поможет нам в
этом – она сделает smartcast объекта

```kotlin
 when (state) {
    State.Loading ->
        println("Loading..")
    is State.Loaded ->
        // в данном случае IDE подскажет, что state - это экземпляр State.Loaded
        println("Success ${state.items}")
    is State.Failure ->
        // и здесь
        println("Failure! ${state.throwable}")
}
```

### Companion object

В джава существуют static final поля, в котлин такого нет, но есть Companion object, в котором можно хранить такие же
свойства

```kotlin
class Counter {
    companion object {
        // аналогично private static final String TAG = "Counter";
        private const val TAG = "Counter"
        // const поддерживает только примитивы
        private const val instance = Counter()
    }
}
```
Здесь следует размещать константы;
константа только примитив!

### Интерфейсы

Интерфейсы не особо отличаются от джава, они стали немного лаконичнее

```kotlin
interface ThemedView {
    fun applyTheme(theme: Theme)
    fun getTheme(): Theme? {
        return null
    }
}
```

Поддерижваются все возможности джава, есть дефолтные реализации методов
Интерфейс может иметь companion object внутри себя

### Enum
Очень мощный инструмент, используется для перечислений, отличий от java нет.
Синтаксис выглядит так:

```kotlin
enum class MimeType(val mimeType: String, val priority: Int) {
    JPEG("jpeg", 1),
    PNG("png", 1),
    MP4("mp4", 7);

    val contentType by lazy {
        "application/$mimeType"
    }

    companion object {
        fun mimeTypeBy(priority: Int): MimeType? {
            return values().firstOrNull { it.priority == priority }
        }
    }
}
```

Поддерживается все что есть в джава, вы так же можете объявлять пропертис и функции внутри companion object

### Data class

Как следует из названия, предназначен для описания объектов данных (например, запрос из сети, объект из БД).

Data class автоматически генерирует equals/hashCode/toString/copy по свойствам, объявленных в primary
constructor.

В java нам бы это пришлось все делать вручную.

```kotlin
data class Image(
    val id: Long = 0,
    val width: Int = 0,
    val height: Int = 0,
    val size: Int = 0,
    val path: String? = null
)
```

Какие ограничения?
– Нельзя наследоваться
– Наличие праймари конструктора с перечисленными пропертями внутри (val, var)
– наличие минимум одной проперти

## Как работать с java и kotlin одновременно

Для этого нам нужно воспользоваться некоторыми аннотациями.

```kotlin
class CustomView @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    @AttrRes defStyleAttr: Int = 0
) : FrameLayout(context, attrs, defStyleAttr) {

    @JvmField
    val index: Int = 0

    companion object {
        @JvmStatic
        fun create(context: Context): CustomView = CustomView(context)
    }
}
```

`@JvmOverloads` - помогает сгенерировать для java несколько конструкторов (их здесь будет 3).
`@JvmField` - позволит обращаться к свойству index напрямую, минуя геттер
`@JvmStatic` - даст нам вызвать метод create как статический без обращения к Companion

## Функции/фичи которые могут пригодится

### Pair

Иногда нужно сгруппировать 2 значения, а создавать для этого целый класс не самая лучшая идея. На помощь приходит класс
Pair. Создание экземпляра Pair очень простое и лаконичное:

```kotlin
val (from, to) = 1 to 20 // будет создана Pair<Int, Int>
// а запись выше называется destructuring declaration 
// from == 1
// to == 20
```

### Полезные глобальные функции 

```kotlin
val list = listOf("1", "a", "ccc")
val map = mapOf(1 to "1", "2" to "2")

val intRange = 1..10
val otherIntRange = 1 until 10

intRange.forEach { i ->
    println(i)
}

for (i in 0..10 step 2) {
    println(i) // 0, 2, 4, 6, 8, 10
}
```

### Форматирование строк

```kotlin
val i = 10
val string = "count = $i"
```

но можно так же поьзоваться и StringBuilder и String.format

### Scope functions: `apply, let, also, with`

```kotlin
class IntentHandler {
    var intent: Intent? = null
    lateinit var view: TextView

    fun scopeFunction() {
        intent = Intent().apply { // apply полезен для создания объектов
            putExtra("name", "value")
        }

        view = TextView()

        with(view) { // полезно для настройки объекта, в данном случае мы задаем свойства нашей вью в ее собственном скоупе
            backgroundColor = Color.BLACK
            text = "Some text"
            textSize = 12
        }

        val i = intent?.let { // let полезен для работы с nullable объектами
            val string = it.extras.getString("1")
            runCathing { string.toInt() }.getOrDefault(5)
        }
    }
}
```
