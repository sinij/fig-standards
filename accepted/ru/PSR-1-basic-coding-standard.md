Основной стандарт написания кода
================================

Этот раздел стандартов включает в себя что следует рассматривать как стандарт
элементов кода, который необходим для обеспечения высокого уровня технической
взаимодействия между общим PHP кодом.

Ключевые слова "ДОЛЖЕН", "НЕ ДОЛЖЕН", "ТРЕБУЕТСЯ", "БУДЕТ", "НЕ БУДЕТ", "СЛЕДУЕТ",
"НЕ СЛЕДУЕТ", "РЕКОМЕНДУЕТСЯ", "МОЖЕТ", и "ДОПОЛНИТЕЛЬНО" в этом документе должны
быть истолкованы как описано в [RFC 2119][].

[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt
[PSR-0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md


1. Обзор
--------

- Файлы ДОЛЖНЫ использовать только `<?php` и `<?=` теги.

- Файлы ДОЛЖНЫ использовать только UTF-8 без BOM для PHP кода.

- Файлам СЛЕДУЕТ *либо* объявлять знаки (классы, функции, константы, и т.д.)
  *или* оказывать побочный эффект (например, генерировать вывод, изменять .ini
  настройки, и т.д.) но НЕ СЛЕДУЕТ делать и то и другое.

- Пространства имен и классы ДОЛЖНЫ следовать [PSR-0][].

- Имена классов ДОЛЖНЫ быть объявлены используя `StudlyCaps`.

- Константы класса ДОЛЖНЫ быть объявлены в верхнем регистре с подчеркиванием 
  в качестве разделителей.

- Имена методов ДОЛЖНЫ быть объявлены используя `camelCase`.


2. Файлы
--------

### 2.1. PHP Теги

PHP код ДОЛЖЕН использовать длинные `<?php ?>` теги или короткие-выводящие 
`<?= ?>` теги; другие варианты тегов НЕ ДОЛЖНЫ использоваться.

### 2.2. Кодировка символов

PHP код ДОЛЖЕН использовать только UTF-8 без BOM.

### 2.3. Побочные эффекты

Файлу СЛЕДУЕТ объявлять знаки (классы, функции, константы, и т.д.),
и не оказывать побочный эффект, или ему СЛЕДУЕТ выполнять логику с побочным 
эффектом, но НЕ СЛЕДУЕТ делать и то и другое.

Фраза "побочны эффекты" означает выполнение логики непосредственно не связанной с
объявлением классов, функций, констант и т.д., *просто от включения
файла*.

"Побочные эффекты" включают, но не ограничены: генерацией вывода, явным 
использованием `require` или `include`, подключением к внешним сервисам, 
изменениям настроек ini, испусканием ошибок или исключений, изменением глобальных 
или статичных переменных, чтением или записью в файл, и так далее.

Ниже приведен пример файла с одновременно объявляющий знаки и оказывающий 
побочные эффекты; т.е., пример того, что следует избегать:

```php
<?php
// побочный эффект: изменение ini настроек
ini_set('error_reporting', E_ALL);

// побочный эффект: загрузка файла
include "file.php";

// побочный эффект: вывод результата
echo "<html>\n";

// описание
function foo()
{
    // тело функции
}
```

В следующем примере файл который содержит объявление без оказания побочных 
эффектов; т.е., пример того, к чему следует стремится:

```php
<?php
// объявление
function foo()
{
    // тело функции
}

// условное объявление это *не* побочный эффект
if (! function_exists('bar')) {
    function bar()
    {
        // тело функции
    }
}
```


3. Пространства имен и Имена Классов
------------------------------------

Пространства имен и классы ДОЛЖНЫ следовать [PSR-0][].

Это означает, что каждый класс находится в собственном файле, и в пространстве 
имен, по крайней мере, на один уровень: верхний уровень имя поставщика.

Имена классов ДОЛЖНЫ быть объявлены используя `StudlyCaps`.

Код написанный на PHP 5.3 и старше ДОЛЖЕН использовать формальные пространства 
имен.

Например:

```php
<?php
// PHP 5.3 и старше:
namespace Vendor\Model;

class Foo
{
}
```

В коде написанном для 5.2.x и раньше СЛЕДУЕТ использовать псевдо-пространства 
имен, условные приставки `Vendor_` к именам классов.

```php
<?php
// PHP 5.2.x и раньше:
class Vendor_Model_Foo
{
}
```

4. Константы, Свойства, и Методы Класса
---------------------------------------

Термин "класс" относится ко всем классам, интерфейсам и трейтам.

### 4.1. Константы

Константы класса ДОЛЖНЫ быть объявлены в верхнем регистре с подчеркиванием в 
качестве разделителей.
Например:

```php
<?php
namespace Vendor\Model;

class Foo
{
    const VERSION = '1.0';
    const DATE_APPROVED = '2012-06-01';
}
```

### 4.2. Свойства

Это руководство намеренно избегает любых рекомендаций, касающихся использования
`$StudlyCaps`, `$camelCase`, или `$under_score` для имен свойств.

Независимо от соглашения именования его СЛЕДУЕТ применять последовательно в 
разумных пределах. Этот предел может быть на уровне поставщика, на уровне 
пакета, на уровне класса, или уровне метода.

### 4.3. Методы

Имена методов ДОЛЖНЫ быть объявлены используя `camelCase`.
