Описание работ
1. Установка и настройка
2. Первая программа
3. Работа с RAM. Переменные
4. Ветвление. Циклы и условия
5. Работа с FPU
6. Работа с SSE/AVX
7. Взаимодействие с C++
8. Вызов функций из C#
9. Разработка графического приложения
10. Разработка загрузчика
11. Взаимодействие с мышью и клавиатурой в реальном режиме
12. Взаимодействие с FAT32. Перечисление и запуск файлов
13. Взаимодействие с FAT32. Чтение и запись файлов
14. Работа с видеоадаптером

# Список команд:

1. Сборка NASM для Windows (x86-64)
```
nasm -f win64 lab_1.asm -o lab_1_x64.o
```

2. Сборка NASM для Windows (x86-32)
```
nasm -f win32 lab_1.asm -o lab_1_x32.o
```

3. Сборка NASM для RealMode (x86-32)
```
nasm lab_1.asm -o lab_1.img
```

4. Линковка в EXE под Windows (x32 или x64). Для X86-32 необходимо в название EntryPoint добавить еще один "_".
```
link lab_1_x64.o /entry:main /subsystem:console /out:lab_1_x64.exe
```

5. Линковка в DLL под Windows (x32 или x64). Пока не понял, почему требуется точка входа.
```
link lab_1_x64.o /entry:main /subsystem:console /out:lab_1_x64.dll /dll
```

6. Линковка для возможности использования stdio:
```
link lab_1_x64.o /entry:main /subsystem:console /out:lab_1_x64.exe legacy_stdio_definitions.lib ucrt.lib
```

7. Просмотр экспортируемых функций:
```
dumpbin /exports lab_1_x64.dll
```
