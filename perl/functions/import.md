﻿# Что такое функция import в Perl?

```perl
import CLASSNAME LIST
import CLASSNAME
```

Встроенной функции <font color="#00aa00">import()</font> не существует.

При подключении модуля с помощью use выполняется две команды: подключение указанного модуля, затем импорт подпрограмм и переменных из него.

Т.е. функция use соответствует коду:

```perl
require My_Module;
import My_Module List_of_sub_and_vars_to_import;
```

Ожидается, что функция <font color="#00aa00">import()</font> находится в подключаемом модуле и содержит в себе инструкции - что и как из этого модуля импортировать. Например, модуль <font color="#00aa00">CGI</font> содержит в себе функцию <font color="#00aa00">import()</font>.

Если нет желания писать <font color="#00aa00">import()</font> для каждого модуля, и нет необходимости в каких-то нестандартных способах импорта, можно использовать модуль <font color="#00aa00">Exporter</font>. Он предоставляет уже готовую для использования функцию
<font color="#00aa00">import()</font>.

Если функции <font color="#00aa00">import()</font> нет в загружаемом модуле, и <font color="#00aa00">Exporter</font> не используется, <font color="#00aa00">use</font> просто пропустит этап импорта переменных.

## Что такое процесс импорта подпрограмм и переменных в Perl?

Процесс импорта подпрограмм и переменных - это процесс создания в текущем пакете псевдонимов для имен подпрограмм и переменных.

Т.е. фактически, в таблице имен текущего пакета создаются записи с именами импортируемых функций и ссылками на их фактическое месторасположение.