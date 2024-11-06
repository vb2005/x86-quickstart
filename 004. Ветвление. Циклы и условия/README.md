# Основы работы с операторами переходов.
В Assembler существуют механизмы изменения прямого хода работы программы. Ветвление здесь не представлено классическими конструкциями if, for или while, однако доступен широкий выбор различных операторов переходов.

Операторы перехода можно условно разделить на 3 группы:
1. Безусловный переход
2. Условные переходы
3. Операторы вызова подпрограмм

# Безусловный переход

Это самый простой вид перехода (команда ```JMP```), он осуществляет переход к некоторой метке внутри программы. В языках программирования он аналогичен оператору Goto

``` cpp
goto label;
...
label:
...
```

Рассмотрим пример
```
jmp _skip_action
...
_skip_action:
...
```
В данном случае, всегда после выполнения оператора ```JMP``` выполнение перейдёт к командам, идущим за меткой ```_skip_action```. Метка в данном случае будет заменен компилятором на смещение (если она располагается рядом) или на абсолютный адрес памяти (если переход требует смены страницы).

# Условный переход

Данный тип перехода подразумевает некоторую проверку регистра флагов. Если условие выпоняется, то будет осуществлен переход к метке, если нет, то продолжится основной ход работы программы. В остальном, принцип работы условного перехода идентичен безусловному.
Принцип работы можно представить следующим кодом:

``` cpp
if (flag) goto label;
...
label:
...
```

Рассмотрим пример:
```
JNE _skip_action
...
_skip_action
...
```

Список всех переходов и принцип их работы представлен ниже:

| КОп | Мнемоника | Описание (на англ.) |
|---|---|---|
|	77 cb	|	JA rel8	|	Jump short if above (CF=0 and ZF=0).	|
|	73 cb	|	JAE rel8	|	Jump short if above or equal (CF=0).	|
|	72 cb	|	JB rel8	|	Jump short if below (CF=1).	|
|	76 cb	|	JBE rel8	|	Jump short if below or equal (CF=1 or ZF=1).	|
|	72 cb	|	JC rel8	|	Jump short if carry (CF=1).	|
|	E3 cb	|	JCXZ rel8	|	Jump short if CX register is 0.	|
|	E3 cb	|	JECXZ rel8	|	Jump short if ECX register is 0.	|
|	E3 cb	|	JRCXZ rel8	|	Jump short if RCX register is 0.	|
|	74 cb	|	JE rel8	|	Jump short if equal (ZF=1).	|
|	7F cb	|	JG rel8	|	Jump short if greater (ZF=0 and SF=OF).	|
|	7D cb	|	JGE rel8	|	Jump short if greater or equal (SF=OF).	|
|	7C cb	|	JL rel8	|	Jump short if less (SF != OF).	|
|	7E cb	|	JLE rel8	|	Jump short if less or equal (ZF=1 or SF != OF).	|
|	76 cb	|	JNA rel8	|	Jump short if not above (CF=1 or ZF=1).	|
|	72 cb	|	JNAE rel8	|	Jump short if not above or equal (CF=1).	|
|	73 cb	|	JNB rel8	|	Jump short if not below (CF=0).	|
|	77 cb	|	JNBE rel8	|	Jump short if not below or equal (CF=0 and ZF=0).	|
|	73 cb	|	JNC rel8	|	Jump short if not carry (CF=0).	|
|	75 cb	|	JNE rel8	|	Jump short if not equal (ZF=0).	|
|	7E cb	|	JNG rel8	|	Jump short if not greater (ZF=1 or SF != OF).	|
|	7C cb	|	JNGE rel8	|	Jump short if not greater or equal (SF != OF).	|
|	7D cb	|	JNL rel8	|	Jump short if not less (SF=OF).	|
|	7F cb	|	JNLE rel8	|	Jump short if not less or equal (ZF=0 and SF=OF).	|
|	71 cb	|	JNO rel8	|	Jump short if not overflow (OF=0).	|
|	7B cb	|	JNP rel8	|	Jump short if not parity (PF=0).	|
|	79 cb	|	JNS rel8	|	Jump short if not sign (SF=0).	|
|	75 cb	|	JNZ rel8	|	Jump short if not zero (ZF=0).	|
|	70 cb	|	JO rel8	|	Jump short if overflow (OF=1).	|
|	7A cb	|	JP rel8	|	Jump short if parity (PF=1).	|
|	7A cb	|	JPE rel8	|	Jump short if parity even (PF=1).	|
|	7B cb	|	JPO rel8	|	Jump short if parity odd (PF=0).	|
|	78 cb	|	JS rel8	|	Jump short if sign (SF=1).	|
|	74 cb	|	JZ rel8	|	Jump short if zero (ZF = 1).	|
|	0F 87 cw	|	JA rel16	|	Jump near if above (CF=0 and ZF=0). Not supported in 64-bit mode.	|
|	0F 87 cd	|	JA rel32	|	Jump near if above (CF=0 and ZF=0).	|
|	0F 83 cw	|	JAE rel16	|	Jump near if above or equal (CF=0). Not supported in 64-bit mode.	|
|	0F 83 cd	|	JAE rel32	|	Jump near if above or equal (CF=0).	|
|	0F 82 cw	|	JB rel16	|	Jump near if below (CF=1). Not supported in 64-bit mode.	|
|	0F 82 cd	|	JB rel32	|	Jump near if below (CF=1).	|
|	0F 86 cw	|	JBE rel16	|	Jump near if below or equal (CF=1 or ZF=1). Not supported in 64-bit mode.	|
|	0F 86 cd	|	JBE rel32	|	Jump near if below or equal (CF=1 or ZF=1).	|
|	0F 82 cw	|	JC rel16	|	Jump near if carry (CF=1). Not supported in 64-bit mode.	|
|	0F 82 cd	|	JC rel32	|	Jump near if carry (CF=1).	|
|	0F 84 cw	|	JE rel16	|	Jump near if equal (ZF=1). Not supported in 64-bit mode.	|
|	0F 84 cd	|	JE rel32	|	Jump near if equal (ZF=1).	|
|	0F 84 cw	|	JZ rel16	|	Jump near if 0 (ZF=1). Not supported in 64-bit mode.	|
|	0F 84 cd	|	JZ rel32	|	Jump near if 0 (ZF=1).	|
|	0F 8F cw	|	JG rel16	|	Jump near if greater (ZF=0 and SF=OF). Not supported in 64-bit mode.	|
|	0F 8F cd	|	JG rel32	|	Jump near if greater (ZF=0 and SF=OF).	|
|	0F 8D cw	|	JGE rel16	|	Jump near if greater or equal (SF=OF). Not supported in 64-bit mode.	|
|	0F 8D cd	|	JGE rel32	|	Jump near if greater or equal (SF=OF).	|
|	0F 8C cw	|	JL rel16	|	Jump near if less (SF != OF). Not supported in 64-bit mode.	|
|	0F 8C cd	|	JL rel32	|	Jump near if less (SF != OF).	|
|	0F 8E cw	|	JLE rel16	|	Jump near if less or equal (ZF=1 or SF != OF). Not supported in 64-bit mode.	|
|	0F 8E cd	|	JLE rel32	|	Jump near if less or equal (ZF=1 or SF != OF).	|
|	0F 86 cw	|	JNA rel16	|	Jump near if not above (CF=1 or ZF=1). Not supported in 64-bit mode.	|
|	0F 86 cd	|	JNA rel32	|	Jump near if not above (CF=1 or ZF=1).	|
|	0F 82 cw	|	JNAE rel16	|	Jump near if not above or equal (CF=1). Not supported in 64-bit mode.	|
|	0F 82 cd	|	JNAE rel32	|	Jump near if not above or equal (CF=1).	|
|	0F 83 cw	|	JNB rel16	|	Jump near if not below (CF=0). Not supported in 64-bit mode.	|
|	0F 83 cd	|	JNB rel32	|	Jump near if not below (CF=0).	|
|	0F 87 cw	|	JNBE rel16	|	Jump near if not below or equal (CF=0 and ZF=0). Not supported in 64-bit mode.	|
|	0F 87 cd	|	JNBE rel32	|	Jump near if not below or equal (CF=0 and ZF=0).	|
|	0F 83 cw	|	JNC rel16	|	Jump near if not carry (CF=0). Not supported in 64-bit mode.	|
|	0F 83 cd	|	JNC rel32	|	Jump near if not carry (CF=0).	|
|	0F 85 cw	|	JNE rel16	|	Jump near if not equal (ZF=0). Not supported in 64-bit mode.	|
|	0F 85 cd	|	JNE rel32	|	Jump near if not equal (ZF=0).	|


# Вызов подпрограмм

Вызов подпрограмм ```call``` имеет сходный с безусловным переходом принцип: перейти к метке. Важным отличием ```call``` от ```jmp``` является запоминание адреса вызова подпрограммы в стек. В результате, после завершения подпрограммы, мы можем вернутся к тому месту, откуда она вызывалась при помощи оператора ```ret```
``` asm
_main:
	call proc1
	call proc2

proc1:
...
	ret

proc2:
...
	ret
```

# Построение конструкции IF ... THEN ... ELSE ...

На основе условных переходов можно построить шаблоны для реализации условного оператора. 

Рассмотрим пример 1. Ветвление без блока ИНАЧЕ
```
ЕСЛИ AX < 0, TO 
AX := 0
```

``` asm
_main:
	test ax, ax		; сравнить AX и AX без записи, т.е. сформировать флаги
	jns _skip_zero	; перейти к метке, если число положительное (нет бита SF)
	mov ax, 0
	_skip_zero:		; переходим сюда по ветке ИНАЧЕ
	ret
```

Рассмотрим пример 2. Ветвление с блоком ИНАЧЕ
```
ЕСЛИ AX < 0, TO 
AX := -1
ИНАЧЕ
AX := 1
```

``` asm
_main:
	test ax, ax		; сравнить AX и AX без записи, т.е. сформировать флаги
	jns _plus		; перейти к метке, если число положительное (нет бита SF)
	mov ax, -1		; Ветка ИНАЧЕ. Меняем значение AX = -1
	jmp _exit		; Выходим из блока проверки
	_plus:			; Ветка ТОГДА
	mov ax, 1		; Меняем значение AX = 1
	_exit:			; Метка завершения блока проверки
	ret
```

# Построение конструкции WHILE ...

Рассмотрим пример 3. Орагнизация цилка с предусловием
```
ПОКА AX < 100
	AX := AX + 1
КОНЕЦ
```

``` asm
_main:

	_loop_start:	; Начало цикла
	test ax, 100	; Сравниваем AX и 100
	jns _loop_end	; AX >= 100? перейти к метке конец цикла
	inc ax			; Действие внутри цикла
	jmp _loop_start ; Переход в начало цикла
	_loop_end:		; Метка завершения цикла
	ret
```

Рассмотрим пример 4. Использование оператора LOOP

В Assembler x86 есть оператор LOOP. Он выполняет условный переход через проверку CX и его декремент

```
ПОКА CX > 0
	...
	CX = CX - 1
КОНЕЦ
```

Для реализации такого цикла используйте следующий код:
``` asm
_loop_start:
  ...				; Некоторые действия
  LOOP _loop_start	; Повторить цикл
```

# Задания на защиту:
1. Реализовать программу по варианту задания. Последовательность задать в блоке .data. Использовать переменные типа DWORD
	1. Найти сумму элементов последовательности
	2. Найти максимальный элемент последовательности
	3. Найти минимальный элемент последовательности
	4. Найти среднее арифметическое элементов последовательности
	5. Найти индекс первого положительного числа последовательности
	6. Найти самую большую разницу между соседними элементами последовательности
	7. Найти индекс максимального элемента последовательности
	8. Найти индекс минимального элемента последовательности

2. Реализовать программу по варианту задания. Последовательность задать в блоке .data. Использовать переменные типа DWORD
	1. Найти последний элемент последовательности Фибоначчи, который умещается в формат uint64
	2. Отсортировать массив в порядке возрастания элементов
	3. Удалить все чётные элементы из последовательности
	4. Удалить все элементы на чётных позициях последовательности

3. Реализовать примеры кода для циклов с параметром. Начальное значение, конечное и шаг можно взять произвольные