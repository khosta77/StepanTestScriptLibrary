# StepanTestScriptLibrary

Библиотека для проведения тестов времени. Произоводит измерение времени выполнения функции, есть возможность
измерить скорость более глубоко, спомощью временных маркеров и их разности. 

## Подключение

```makefile
-include StepanTestScriptLibrary/stepantestscriptlibrary.mk
```

Добавить *$(OBJS) makefile* при сборке

## Применение

### Добавить заголовок

```C++
#include "StepanTestScriptLibrary.h"
```

### Тестовая функция

Проверка все функции

```C++
double work() {
    // Работа
    return 0;
}
```

Проверка части или метода/функции, которой нужна долга инициализация

```C++
double work() {    
    double s1, e1, s2, e2;

    s1 = STSL::getCPUTime();

    // Инициализация

    e1 = STSL::getCPUTime();

    // Работа

    s2 = STSL::getCPUTime();

    // Очистка, если надо

    e2 = STSL::getCPUTime();
    
    return ((e2 - s2) + (e1 - s1));
}
```

### Инициализация теста

```C++
void test() {
    // Создание объекта тестирования
    STSL::TimeTest tt(std::string("Имя процессора/системы.csv"));

    // Группы рекомендую выделять в отдельную переменную
    const std::string group_name = "";

    // Пример добавления тестов
    tt.add(
        group_name,
        "test_name",
        work
    );

    // Для удобства стоит сохранить переменную с результатами
    auto rtt = tt.dotests();

    // Вывести 1 тест
    STSL::DataFrameFunction::print_pair_resultdf(rtt);

    // Для вывода в csv или tex нужен другой объект
    STSL::ResultsOut ro(rtt);

    // Вывод в формат tex
    ro.outTexResults();

    // Вывод в сsv
    ro.writeResultsToCsv();


    STSL::ResultsOut ro2;

    // Чтение из csv
    ro2.readResultFromCsv("Имя процессора/системы.csv");

    // Вывод считанного.
    ro2.outTexResults();
}
```

## Пример

### Пример работы статус бар

Слева с скобках указана группа готорая тестируется

```cmd
Тестирование: Проверка скорости работы функции getCPUTime()
[1/2][============================================================] 100 %
Тестирование: Тест 1, на 100 элементов
[2/2][============================================================] 100 %
Тестирование: Тест 2, на 1000 элементов
[2/2][============================================================] 100 %
Тестирование: Тест 3, на 10000 элементов
[2/2][=================>                                          ] 28 %
```

### Пример вывод в терминал

```cmd
CPU: Apple Silicon M1 Pro 14+8
Группа: Проверка скорости внутренних функций, нужных для уточнения погрешности
Имя теста: Проверка скорости работы функции getCPUTime()
---> avg: -0.001849 min: -0.002036 max: -0.001656



Группа: Проверочная группа
Имя теста: Тест 1, на 100 элементов
---> avg: 0.000054 min: 0.000052 max: 0.000064

Имя теста: Тест 2, на 1000 элементов
---> avg: 0.002696 min: 0.002524 max: 0.003390

Имя теста: Тест 3, на 10000 элементов
---> avg: 0.239396 min: 0.236820 max: 0.240950
```

### Пример вывод в формате tex

```tex
\begin{tabular}{|p{2.0in}|p{1.5in}|p{1.5in}|}
    \hline
    Название & Времнная & \multicolumn{1}{c|}{Процессор}  \\ \cline{3-3}
    теста    & характеристика & Apple Silicon M1 Pro 14+8 \\
    \hline
    \multicolumn{3}{|c|}{Проверка скорости внутренних функций, нужных для уточнения погрешности} \\
    \hline
    Проверка скорости работы функции getCPUTime() & avg & -0.001849\\ \cline{2-3}
                                                  & min & -0.002036\\ \cline{2-3}
                                                  & max & -0.001656\\
    \hline 
    \multicolumn{3}{|c|}{Проверочная группа}     \\
    \hline
    Тест 1, на 100 элементов & avg & 0.000054    \\ \cline{2-3}
                             & min & 0.000052    \\ \cline{2-3}
                             & max & 0.000064    \\
    \hline 
    Тест 2, на 1000 элементов & avg & 0.002696   \\ \cline{2-3}
                              & min & 0.002524   \\ \cline{2-3}
                              & max & 0.003390   \\
    \hline 
    Тест 3, на 10000 элементов & avg & 0.239396  \\ \cline{2-3}
                               & min & 0.236820  \\ \cline{2-3}
                               & max & 0.240950  \\
    \hline 
\end{tabular}
```

# Версия 1.0
