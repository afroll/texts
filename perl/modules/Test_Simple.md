﻿# Test::Simple

**Test::Simple** - это базовый, очень простой модуль, который используется для написания тестов.

Модуль дает возможность использовать для тестирования всего одну функцию - **ok()**. Если возможностей данной
функции недостаточно, рекомендуется использовать Test::More. Тесты, написанные с помощью Test::Simple полностью
совместимы с Test::More.

Вывод результатов тестирования производится в формате TAP (Test Anything Protocol).

### Подключение Test::Simple

При подключении Test::Simple следует заранее сообщать программе сколько тестов планируется выполнить:

```perl
use Test::Simple tests => 23;
```

Под количеством тестов подразумевается то, сколько будет запущено специальных тестирующих функций во время выполнения программы.

Например, сколько раз будет запущена функция ок().

Если указанное число и число реально выполненных тестов не будет совпадать, пользователю будет выведена ошибка.

**Пример (Выполнение одного теста в программе, при заранее заданных 2х):**

```perl
#!/usr/bin/perl

use Test::Simple tests => 2;

ok(1+1 == 2,'1+1=2');
```

**Вывод результатов тестирования:**

<pre>
%perl test_simple.pl
1..2
ok 1 - 1+1=2 # Looks like you planned 2 tests but only ran 1.
</pre>

При запуске тестирующей программы Test::Simple выводит строку формата "1.. М", где М - это число тестов, которое предполагается выполнить в процессе тестирования.

### ок()

**ок()** - основная и единственная тестирующая функция, предоставляемая Test::Simple. Позволяет проверить успешность выполнения Вашей программы, функции, части программного кода.

**Синтаксис ok():**

<pre>ok( $test_var eq $ok_value, 'test_var eq ok_value' );</pre>

Функция обрабатывает переданное ей условное выражение. Если результат обработки положителен (true), тест будет считаться пройденным. В зависимости от результата, функция выведет сообщение "ok" или "not ok" с порядковым номером теста.

В качестве второго аргумента функции, можно указывать краткое описание проводимых тестов. При выводе результатов тестирования, указанное описание будет выводиться в одну строку с результатами выполнения конкретного теста.

**Пример:**

```perl
#!/usr/bin/perl

use Test::Simple tests => 1;

ok(1+1 == 2,'Summation 1+1');
```

**Вывод программы:**
<pre>%perl test_simple.pl
1..1
ok 1 - Summation 1+1
</pre>

Использование подобных кратких описаний облегчает задачу поиска нужных строк в программном
коде и внесения исправлений. Кроме того, описания полезны при разработке и использовании тестов в команде.
