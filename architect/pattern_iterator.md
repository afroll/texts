﻿# Шаблон проектирования Iterator

Итератор - паттерн поведения объектов.

Предоставляет способ последовательного доступа ко всем элементам составного объекта, не раскрывая его внутреннего представления. Известен также под именем <font color="#00aa00">Cursor</font> (курсор).

Составной объект, скажем список, должен предоставлять способ доступа к своим элементам, не раскрывая их внутреннюю структуру. Более того, иногда требуется обходить список по-разному, в зависимости от решаемой задачи. Все это позволяет сделать итератор.

Итератор определяет интерфейс для доступа к элементам структуры данных и отслеживает текущий элемент, чтобы иметь представление, какие элементы уже посещались.

## Пример итератора для Perl

Perl имеет отличные встроенные возможности для работы со списками, массивами, хешами и соответствующими объектами.

Большинство программистов даже не замечают, что используют итераторы постоянно. Например, используя <font color="#00aa00">foreach</font> для поэлементной обработки массива. При этом, всегда можно перейти к обработке следующего элемента (<font color="#00aa00">next</font>), остановить итеративный цикл (<font color="#00aa00">last</font>), повторно обработать текущий элемент (<font color="#00aa00">redo</font>).

Доступны операторы <font color="#00aa00">for</font>, <font color="#00aa00">foreach</font>, <font color="#00aa00">++</font>, регулярные выражения, <font color="#00aa00">&lt;&gt;</font> для последовательного чтения строк в файле, <font color="#00aa00">map</font>, и пр. - все они так или иначе реализуют идею итератора.

```perl
for my $color ( $picture->colors() ) {
    #...
}
```

Чаще всего, специальные итераторы приходится писать только к собственным сложным структурам данных, к элементам которых требуется получить последовательный доступ. Каждый итератор должен, как минимум, знать, как получить доступ к следующему элементу, и что делать, если достигнут конец структуры данных.

Например, специальные итераторы активно используются при работе с <font color="#00aa00">DBI</font>:

```perl
my $sth = $dbh->execute($sql);
while ( my $row = $sth->fetchrow_hashref() ) {
    ...
}
```

С помощью метода <font color="#00aa00">fetchrow_hashref</font> происходит извлечение следующей строки из выполненной выборки в таблицах БД. Процесс извлечения записей можно прервать в любой момент, инициировав выход из цикла <font color="#00aa00">while</font>.

## Простой пример реализации итератора

Итератор возвращает следующий элемент массива. Когда элементов больше не остается, возвращает <font color="#00aa00">undef</font>.

Реализация итератора:

```perl
package Pack;

use strict;

sub new {
  my $type=shift;
  my $self;
  my $data = shift;
  $data = [] unless $data=~/ARRAY/;

  $self = bless( {
    iterator => 0,
    data => $data
  }, $type);
  return $self;
}
sub fetch {
  my $self = shift;
  my $element;

  if ($self->{iterator} &lt; @{$self->{data}}) {
    $element = $self->{data}->[$self->{iterator}];
    $self->{iterator}++;
  } else {
    $element = undef;
  }
  return $element;  
}
1;
```

Использование итератора:

```perl
#!/usr/bin/perl

use strict;
use Pack;

my $object = Pack->new(['Element1', 'Element2', 'Element3', 'Element4']);
while (my $item = $object->fetch) {
	print $item."\n";
}
exit;
```

