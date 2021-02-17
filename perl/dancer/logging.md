# Dancer. �����������

��-���������, ����� ���������, Dancer ������������ ��� ���� ����������� �� �����: ������ ����� � ���� ��� �����
����������� ������ �� �������. ����� ��������� ���������� ���������� ������ �������.

��� ���������� ����� � �����, ��� ������� ��������� � ������� ��� ����������� �������������, ���� ������
� ���������� ��������� ���������:
<ol>
<li>� �������� "environments" ������������ ��������� ��������, ��� ���������� ���������. � ����������� �� ����, ����� ��������� �� �����������, � ��������������� ����� ���� ��������� �������� logger � "console" �� "file".
<pre>logger: "file"</pre>
</li>

<li>��������� ������� ����������� ������ �����. ��-���������, � ������� �����������:
<pre>log: "core"</pre>
����� ������� ���� �� ����� �����������:
<pre>log: 'debug'     # ����� �������� ���: ������, ��������������, ���������� 
                 # � �������������� ���������
log: 'info'      # ����� �������� ������, ��������������, �������������� 
                 # ���������
log: 'warning'   # ����� �������� ������ �������������� � ������
log: 'error'     # ������� ������ ������
</pre>
</li>

<li>��������� ������ �����. � ����� �������, ��� ������� ��� ����� ����������� (������ "��-���������" �� ������):
<pre>[1900]  core @0.021200&gt; [hit #1]response: 200 in 
&gt; C:/strawberry/perl/site/lib/Dancer/Handler.pm l. 179
</pre>
, � ������ - ������� ���� �����������������, ��� �������� ��������� �������������� ����������� ��� �����, ����������
������� �����������. ������ ������������� � ��� �� �����, ����� �� �������� �������� "environments".

������:
<pre>logger_format: "[%T] [%P] %f:%l %m"</pre>

������� � ��� ������:
<pre>[2012-10-08 11:51:03] [2788] C:/strawberry/perl/site/lib/Dancer/Route.pm:102  
 --&gt; got 1
</pre>
</li>

<li>������������� ������.
����� ������, ������ ������� ����� "logs" � ����� �������, ������� ����� ��������� ����� �����. ��� ������� ���������
����� �������������� ����������� ����������� ����.</li>
</ol>