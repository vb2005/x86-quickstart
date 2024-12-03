# Знакомство с Raspberry Pi

Перед Вами портативный компьютер Rasperry Pi 4. Это полноценный ПК на базе процессора с архитектурой ARM (Aarch64). Он имеет следующие характеристики: 

```
CPU Quad Core Cortex-A72 (ARM v8) 64-bit SoC @ 1.5GHz
4 ГБ LPDDR4–3200 SDRAM
Wi-Fi 2.4/5.0 ГГц, Bluetooth 5.0, BLE
1GigE port
2x USB 3.0 и 2x USB 2.0
2x micro-HDMI (4K p60)
2x MIPI DSI display port
2x MIPI CSI camera port
Аппаратное декодирование видео до 4K H.265 (4kp60 decode), H264 (1080p60 decode, 1080p30 encode)
```

Интересным решением тут является отсуствие ПЗУ. В роли накопителя здесь выступает карта MicroSD с EFI загрузчиком, который программируется заранее при помощи ПК.

Также микроПК оборудован шиной GPIO, которая поддерживает следующие интерфейсы передачи данных:

```
21x GPIO, 2x ШИМ, 2x I2C, 2x SPI, UART
```

Таким образом, Raspberry Pi сочетает в себе как классические интерфейсы ПК, так и интерфейсы микроконтроллеров, и позволяет решать задачи, связанные со взаимодействием различных устройств.

Разработка программ для микрокомпьютера может идти как на низком уровне (без контекста операционной системы), так и на высоком (под контекстом ОС Linux (Raspbian, Ubuntu), ОС Windows IoT). 

Сегодня мы позакомимся с разработкой косольных программ под ОС Raspbian (ныне Raspberry Pi OS)

## Подготовка к работе

Для выполнения лабораторной работы Вам потребуются:

1. Микрокомпьютер
2. Блок питания (USB-C PD или USB-C 3A и выше)
3. Карта MicroSD с ОС Raspbian
4. Переходник Micro-HDMI -> VGA
5. Монитор VGA-совместимый
6. Мышь, клавиатура
7. Доступ в Интернет (опционально)
8. Адаптер USB-MicroSD (опционально)

Подключите монитор, вставьте карту MicroSD, мышь, клавиатуру, сетевой провод к микроПК. Последним подключите питания Raspberry

Если все подключено корректно, загрузится интерфейс рабочего стола Linux.

## Разработка программы на C

1. На рабочем столе создайте каталог с номером своей группы
2. Перейдите в каталог и создайте в нём файл ```app1.c```
3. В файл добавьте программный код

``` cpp
#include <stdio.h>

int main (void)
{
	printf ("Hello World\n");
}
```

4. Сохраните файл
5. Теперь откройте командную строку (терминал). Сделать это можно клавишей ![](https://img.shields.io/badge/F4-blue)
6. Выполните команду сборки
```
gcc app1.c -o app1
```
7. Запустие файл командой
```
./app1
```

## Разработка программы на C++

1. В своём каталоге создайте файл ```app2.cpp```
2. В файл добавьте программный код

``` cpp
#include <iostream>

using namespace std;

int main()
{
    int *arr; // указатель для выделения памяти под массив
    int size; // размер массива
    
    // Ввод количества элементов массива
    cout << "n = ";
    cin >> size;

    if (size <= 0) {
        // Размер масива должен быть положитлеьным
        cerr << "Invalid size" << endl;
        return 1;
    }

    arr = new int[size]; // выделение памяти под массив

    // заполнение массива
    for (int i = 0; i < size; i++) {
        cout << "arr[" << i << "] = ";
        cin >> arr[i];
    }

    int temp; // временная переменная для обмена элементов местами

    // Сортировка массива пузырьком
    for (int i = 0; i < size - 1; i++) {
        for (int j = 0; j < size - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                // меняем элементы местами
                temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }

    // Вывод отсортированного массива на экран
    for (int i = 0; i < size; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;

    delete [] arr; // освобождение памяти;
    
    return 0;
}
```

3. Сохраните файл
4. Теперь откройте командную строку (терминал). Сделать это можно клавишей ![](https://img.shields.io/badge/F4-blue)
5. Выполните команду сборки
```
g++ app2.cpp -o app2
```
7. Запустие файл командой
```
./app2
```

# Сборка под Linux с поддержкой Python + OpenCV

1. Устанвливаем OpenCV

Следующие команды: 

``` 
sudo apt install libgtk-3-dev libboost-all-dev libatlas-base-dev libavcodec-dev libavformat-dev libswscale-dev libv4l-dev libxvidcore-dev libx264-dev libjpeg-dev libpng-dev libtiff-dev gfortran openexr libopenexr-dev python3-dev python3-numpy
mkdir ~/opencv_build && cd ~/opencv_build
git clone https://github.com/opencv/opencv.git
git clone https://github.com/opencv/opencv_contrib.git
cd ~/opencv_build/opencv
mkdir build && cd build
apt install cmake

cmake -D CMAKE_BUILD_TYPE=RELEASE \
      -D CMAKE_INSTALL_PREFIX=/usr/local \
      -D INSTALL_C_EXAMPLES=ON \
      -D INSTALL_PYTHON_EXAMPLES=ON \
      -D OPENCV_GENERATE_PKGCONFIG=ON \
      -D OPENCV_EXTRA_MODULES_PATH=~/opencv_build/opencv_contrib/modules \
      -D BUILD_EXAMPLES=ON ..

make -j$(nproc)

sudo make install
sudo sh -c 'echo "/usr/local/lib" > /etc/ld.so.conf.d/opencv.conf' 
sudo ldconfig
```

2. Собираем приложение

Без заголовочного файла и pch.h. Весь код пихаем в cpp-файл. В нём также маркируем функции через ```extern "C"```

Запускаем следующую команду:
```
gcc -fPIC -Wall -o3 -shared dip.cpp -o app.so `pkg-config --cflags --libs opencv4`
```

Проверяем наличие функций и адекватность их нейминга через 
``` nm app.so ```

Наши функции должны быть с флагом ```T```

3. Переходим в Python

Проверяем разрядности питона и библиотки (у меня x86_64)
Пробуем подключить библиотеку этим MVP:
``` python
import ctypes
l = ctypes.CDLL('/home/kvantron/app.so')
```

Если все ок, значит ок. Если ошибки - смотри пути, разрядности и наличие зарегистрированых библиотеку

4. Прописываем функции

Важный момент - нужно все аргументы и возвращаемые типы правильно описать

Для такой функции
``` python 
PointerReturnGetTyreRectImageWithContrasting
   # unsigned char* src,
   # int w,
   # int h,
   # float contrast_procentile,
   # int min_diameter_px,
   # int max_diameter_px,
   # int hough_detect_param_1,
   # int hough_detect_param_2)

   # return unsigned char* 
```

Такое описание:
``` python 
l.PointerReturnGetTyreRectImageWithContrasting.argtypes = [
                        np.ctypeslib.ndpointer(dtype=np.uint8, ndim=1, flags='C_CONTIGUOUS'), 
                        ctypes.c_size_t, 
                        ctypes.c_size_t, 
                        ctypes.c_float, 
                        ctypes.c_size_t, 
                        ctypes.c_size_t, 
                        ctypes.c_size_t, 
                        ctypes.c_size_t]

l.PointerReturnGetTyreRectImageWithContrasting.restype = ctypes.POINTER(ctypes.c_byte)
```

5. Вызывавем. Тут возвращаемое значение нужно формировать как указатель и кастить к массиву Numpy(uint8)
Вот мой MVP:
``` python
import ctypes
import cv2
import numpy as np
x = cv2.imread('Cat03.jpg', cv2.IMREAD_GRAYSCALE)

print(x.dtype)

l = ctypes.CDLL('/home/kvantron/app.so')

l.PointerReturnGetTyreRectImageWithContrasting.argtypes = [
                        np.ctypeslib.ndpointer(dtype=np.uint8, ndim=1, flags='C_CONTIGUOUS'), 
                        ctypes.c_size_t, 
                        ctypes.c_size_t, 
                        ctypes.c_float, 
                        ctypes.c_size_t, 
                        ctypes.c_size_t, 
                        ctypes.c_size_t, 
                        ctypes.c_size_t]

l.PointerReturnGetTyreRectImageWithContrasting.restype = ctypes.POINTER(ctypes.c_byte)



result = l.PointerReturnGetTyreRectImageWithContrasting(x.ravel(), 799, 800, 0.5, 1, 1, 1, 1)

arr = np.ctypeslib.as_array(result, shape=(800*799,))
arr = arr.reshape((799,800))
arr = arr.astype('uint8')
print(arr.dtype)

cv2.imwrite('123.png',arr)
```
