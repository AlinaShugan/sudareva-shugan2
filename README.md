В данной работе проводится анализ данного временного ряда, а так же осуществляется попытка предсказать значения для последующих месяцев.

Задание:
=======================================

1.	Считать данные из training.csv. Проверить является ли ряд стационарным в широком смысле. Это можно сделать двумя способами:

    1.	Провести визуальную оценку, отрисовав ряд и скользящую статистику(среднее, стандартное отклонение). Постройте график на котором будет отображен сам ряд и различные скользящие статистики.

    2.	Провести тест Дики - Фуллера.

    Сделать выводы из полученных результатов. Оценить достоверность статистики. 

2.	Разложить временной ряд на тренд, сезональность, остаток в соответствии с аддитивной, мультипликативной моделями. Визуализировать их, оценить стационарность получившихся рядов, сделать выводы. 

3.	Проверить является ли временной ряд интегрированным порядка k. Если является, применить к нему модель ARIMA, подобрав необходимые параметры с помощью функции автокорреляции и функции частичной автокорреляции. Выбор параметров обосновать. Отобрать несколько моделей. Предсказать значения для тестовой выборки. Визуализировать их, посчитать r2 score для каждой из моделей. Произвести отбор наилучшей модели с помощью информационного критерия Акаике. Провести анализ получившихся результатов. 

Доп. задания:
* соблюдение PEP8
* использование для визуализации библиотеки seaborn. 

Описание программы:
======================================
Сначала с помощью matplotlib и seaborn строится график данного в задании временного ряда.

Далее осуществляеся проверка на стационарность временного ряда с помощью функции test_stationarity, на вход которой подается ряд и строка с названием. Внутри test_stationarity  с помощью функций Series.rolling.mean() и Series.rolling.std() находятся среднее(rolmean) и стандартное отклонение(rolstd) соответственно, строятся их графики вместе с оригинальным, а затем проводится тест Дики-Фуллера с помощью функции adfuller из модуля pandas. 

Разложение ряда на тренд, сезонность и остаток производится с помощью функции seasonal_decompose из модуля statmodels с параметрами model='additive' и model='multiplicative' для аддитивной и мультипликативной моделей соответственно. Далее идет проверка на стационарность каждого из полученных рядов описанным выше способом.

Выводы:
======================================
Оригинальный ряд и трендовая составляющая для обоих моделей не стационарны, т.к. стандартное отклонение и скользящее среднее зависят от времени. Тест Дики-Фуллера подтверждает данный вывод. 

Сезонная составляющая и остаток (для обоих моделей) стационарны, т.к. ряды являются константами, стандартное отклонение и скользящее среднее не зависят от времени. 

Необходимое ПО
=========================
Windows 7

Необходимые программы (библиотеки):
=========================
Используемые библиотеки:
-------------------------------
* `Pandas` - данный пакет делает Python мощным инструментом для анализа данных. Пакет дает возможность строить сводные таблицы, выполнять группировки, предоставляет удобный доступ к табличным данным, а при наличии пакета matplotlib дает возможность рисовать графики на полученных наборах данных.
* `Statsmodels` - это модуль Python, который предоставляет классы и функции для оценки различных статистических моделей, а также для проведения статистических тестов и исследования статистических данных.
* `Matplotlib` - модуль Python, необходимый для визуализации решений и простроения графиков. 
* `Seaborn` - это библиотека визуализации Python на основе matplotlib.

Программы:
----------------------
Python 3.6

Anaconda3

Jupyter 

Инструкция к запуску
======================
Здесь и далее будем считать, что пакет Anaconda установлен в Windows, в папку C:\Anaconda3, в Linux, вы его можно найти в каталоге, который выбрали при установке.

Перейдите в папку Scripts и введите в командной строке:

ipython notebook 

Если вы находитесь в Windows и открыли папку C:\Anaconda3\Scripts через проводник, то для запуска интерпретатора командной строки для этой папки в поле адреса введите cmd. 

В результате запустится веб-сервер и среда разработки в браузере. 

Выбираем файл task2.ipynd и запускаем его. При этом файл testing.csv должен находиться в той же папке, что и task2.ipynd. 

Участники
======================
**Шуган Алена - 312 гр.**

1, 2 часть задания + визуализация решений с помощью библиотеки seaborn

**Сударева Валерия -  312 гр.**

3 часть задания
