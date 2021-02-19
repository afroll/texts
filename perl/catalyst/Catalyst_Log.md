﻿# Catalyst::Log

Класс Catalyst для работы с логами. Используется Catalyst по-умолчанию. Если вы хотите использовать другой класс, нужно будет указать в системе:

<pre>$c->log(MyLogger->new);</pre>

Следует учитывать, что ваш класс должен будет реализовывать аналогичный Catalyst::Log интерфейс.

## Уровни логирования

У каждого log-сообщения есть уровень его "важности". Именно этот уровень называют "log level". Обычно выделяют следующие уровни логирования: critical, error, warning, info, debug. В разных системах могут отличаться названия уровней (заменяются на синонимы или используются сокращения), но общий принцип сохраняется.

Иногда, в зависимости от того, какой уровень важности назначен выводимому сообщению,
приложение может менять свое поведение. Например, можно разделять потоки вывода логов: сообщения уровня info и warning писать в лог на сервере, а для critical - отправлять email и sms отделу разработки.

## Уровни логирования в Catalyst::Log

<ul>
<li><b>debug</b> - отладочные сообщения, информация для дальнейшего использования в разработке. Очень удобно выводить дампы наборов данных. Вызов $log-&gt;debug() никак не влияет на приложение и процесс его работы. Данный уровень практически никогда не используется для приложений, запущенных на боевых серверах.
<pre>$log->is_debug;
$log->debug($message);
$c->log->debug($message);
</pre>
</li>

<li><b>info</b> - общая информация.
<pre>$log->is_info;
$log->info($message);
$c->log->info($message);
</pre>
</li>

<li><b>warn</b> - информация о мелких проблемах и ошибках, которые возникают при работе приложения. Как правило, подобные ошибки не влияют на основную работоспособность приложения, не вызывают отказа системы.
<pre>$log->is_warn;
$log->warn($message);
$c->log->warn($message);
</pre>
</li>

<li><b>error</b> - информация об ошибках в работе приложения.
<pre>$log->is_error;
$log->error($message);
$c->log->error($message);
</pre>
</li>

<li><b>fatal</b> - информация о критических событиях. Если возникла критическая ошибка, то в результате, вероятнее всего, процесс умирает или очень близок к этому. Более того, часто ошибки подобного уровня отлавливаются приложением, и после этого сразу вызывается die или что-то аналогичное.
<pre>$log->is_fatal;
$log->fatal($message);
$c->log->fatal($message);
</pre>
</li>
</ul>

С другой стороны, разделение ошибок на категории error и critical - достаточно субъективно.

## Методы Catalyst::Log
**new**

Конструктор. По умолчанию включает работу со всеми уровнями. Если предполагается работа только с некоторыми из уровней, нужно указать их списком в new().
<pre>
$log = Catalyst::Log->new;
$log = Catalyst::Log->new( 'warn', 'error' );
</pre>
Объект Catalyst::Log не хранит список установленных уровней, он сохраняет только производную битовую маску.

&nbsp;

**level**

level - работает с битовой маской установленных уровней. Может вернуть маску, или установить. На основе данного метода реализуют свою функциональность методы levels(), enable() и disable().

Пример:
<pre>$c->log->debug("LABEL: ".$c->log->level);

# В лог-файле можно будет найти примерно такую строку:
[debug] LABEL: 31
</pre>
&nbsp;

**levels**

Если желаемые уровни не были указаны на этапе создания нового объекта, можно сделать это позднее с помощью метода levels(). Метод levels() задаст уровни заново, обнулив предыдущие настройки, если они были:
<pre>$log->levels( 'warn', 'error', 'fatal' );
</pre>
Метод levels() предназначен только для установки значений, не для чтения. Если вы попробуете вызвать $c-&gt;log-&gt;levels(), то вместо вывода имеющихся настроек, метод подумает, что его вызвали с пустым списком параметров, и отключит все уровни логирования.


**enable**

Добавляет указанные уровни. В отличие от levels(), который выполняет замену настроек, метод enable() дополняет их.
<pre>$log->enable( 'warn', 'error' );
</pre>
&nbsp;

**disable**

Деактивирует указанные уровни. Выполняет действие, противоположное тому, какое выполняет enable().
<pre>$log->disable( 'warn', 'error' );
</pre>
Если в каком-нибудь action указать такой код:
<pre>$c->log->debug("LABEL: some text debug");
$c->log->warn("LABEL: some text warn");

$c->log->disable( 'warn' );
$c->log->debug("LABEL: some text debug 2");
$c->log->warn("LABEL: some text warn 2");
</pre>

то в логе будет вот такой вывод:
<pre>[debug] LABEL: some text debug
[warn] LABEL: some text warn
[debug] LABEL: some text debug 2
</pre>
С помощью метода disable() были деактивированы уведомления типа "warning", и последующий вызов $c-&gt;log-&gt;warn() будет проигнорирован.

Во время отладки, используя $c-&gt;log-&gt;debug(), я часто начинаю вывод с приставки типа "LABEL:" . Написать можно что угодно, "WWW:" или "DDD:" - просто несколько символов и двоеточие. Вывод в лог достаточно объемный, и благодаря установленным меткам, со строго определенным форматом, потом очень легко найти нужные отладочные сообщения в файле. Не надо будет его пролистывать, можно просто использовать поиск. Кроме того, такие метки хорошо заметны. Перед выкладкой кода на боевые сервера, естественно, все эти сообщения вычищаются.

**is_debug**

**is_error**

**is_fatal**

**is_info**

**is_warn**

Перечисленные методы проверяют, включены ли соответствующие уровни.

**abort**

Будет сбрасывать логи в конце каждого запроса.

**autoflush**

По-умолчанию, сообщения в лог catalyst-а выводятся в тот момент, когда вызывается соответствующий метод для отправки сообщения. Можно изменить это поведение с помощью autoflush(). Передав значение "0" в качестве аргумента, мы получим поведение логгера, когда все сообщения накапливаются в специальной очереди и выводятся всей массой только когда выполнится запрос.

Сочетая autoflush и abort можно полностью подавлять вывод логов.

**_send_to_log**

<pre>$log->_send_to_log(@messages);</pre>

Защищенный метод, который посылает информацию в STDERR. При необходимости можно создать подкласс и переопределить этот метод.