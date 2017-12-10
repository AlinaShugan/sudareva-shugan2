В данной работе проводится анализ данного временного ряда, а так же осуществляется попытка предсказать значения для последующих лет.

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

Вводные понятия:
======================================
Под *временным рядом* понимаются последовательно измеренные через некоторые (зачастую равные) промежутки времени данные.
*Прогнозирование временных рядов* заключается в построении модели для предсказания будущих событий, основываясь на известных событиях прошлого.
*Ана́лиз временны́х рядо́в* — совокупность математико-статистических методов анализа, предназначенных для выявления структуры временных рядов и для их прогнозирования.

Ряд называется *слабо стационарным*  или *стационарным в широком смысле*, если его среднее значение и дисперсия не зависят от времени, а ковариационная функция зависит только от сдвига. Если нарушается хотя бы одно из этих условий, то ряд является нестационарным.

*Тест Дики — Фуллера*  — это методика, которая используется в прикладной статистике и эконометрике для анализа временных рядов для проверки на стационарность. Является одним из тестов на единичные корни (Unit root test).

Как и большинство других видов анализа, анализ временных рядов предполагает, что данные содержат *систематическую составляющую* (обычно включающую несколько компонент) и *случайный шум* (ошибку), который затрудняет обнаружение регулярных компонент. 
Большинство регулярных составляющих временных рядов принадлежит к двум классам: они являются либо трендом, либо сезонной составляющей. 

*Тренд* представляет собой общую систематическую линейную или нелинейную компоненту, которая может изменяться во времени. 
*Сезонность* – строго периодические и связанные с календарным периодом отклонения от тренда:

* *Аддитивная сезонность* – амплитуда сезонных колебаний не имеет ярко выраженной тенденции к изменению во времени.
* *Мультипликативная сезонность* – амплитуда сезонных колебаний имеет выраженную тенденцию к изменению во времени.


Описание программы:
======================================
Сначала с помощью matplotlib и seaborn строится график данного в задании временного ряда.

Далее осуществляеся проверка на стационарность временного ряда с помощью функции test_stationarity, на вход которой подается ряд и строка с названием. Внутри test_stationarity  с помощью функций Series.rolling.mean() и Series.rolling.std() находятся среднее(rolmean) и стандартное отклонение(rolstd) соответственно, строятся их графики вместе с оригинальным, а затем проводится тест Дики-Фуллера с помощью функции adfuller из модуля pandas. 

Разложение ряда на тренд, сезонность и остаток производится с помощью функции seasonal_decompose из модуля statmodels с параметрами model='additive' и model='multiplicative' для аддитивной и мультипликативной моделей соответственно. Далее идет проверка на стационарность каждого из полученных рядов описанным выше способом.

Ряд проверяется на интегрируемость порядка k. Поэтому для проверки проверки стационарности давайте проведем обобщенный тест Дикки-Фуллера на наличие единичных корней. Для этого в модуле statsmodels есть функция adfuller().Если проведенный тест подтвердил предположения о не стационарности ряда. Для нахождения k будем брать разности рядов . Если, например, первые разности ряда стационарны, то он называется интегрированным рядом первого порядка.
В нашем случае k = 1, а ряд интегрируем.
Если ряд интегрируем, то с помощью функции автокорреляции и функции частичной автокорреляции подбираются необходимые параметры для модели ARIMA.
Итак, чтобы построить модель нам нужно знать ее порядок, состоящий из 3-х параметров:
p — порядок компоненты AR
d — порядок интегрированного ряда
q — порядок компонетны MA

Параметр d есть и он равет 1, осталось определить p и q. Для их определения нам надо изучить авторкорреляционную(ACF) и частично автокорреляционную(PACF) функции для ряда первых разностей.
ACF поможет нам определить q, т. к. по ее коррелограмме можно определить количество автокорреляционных коэффициентов сильно отличных от 0 в модели MA
PACF поможет нам определить p, т. к. по ее коррелограмме можно определить максимальный номер коэффициента сильно отличный от 0 в модели AR.
Чтобы построить соответствующие коррелограммы, в пакете statsmodels имеются следующие функции: plot_acf() и plot_pacf(). Они выводят графики ACF и PACF, у которых по оси X откладываются номера лагов, а по оси Y значения соответствующих функций.
После изучения коррелограммы PACF можно сделать вывод, что p = 1, т.к. на ней только 1 лаг сильно отличнен от нуля. По коррелограмме ACF можно увидеть, что q = 1, т.к. после лага 1 значении функций резко падают.

Далее строятся модели ARIMA и SARIMAX и осуществляется прогноз. Строится график, на котором изображены данные из файла testing.csv и построенный прогноз для каждой из моделей (пунктиром). Для каждой модели считается R2 - коэффициент детерминации, чтобы понять какой процент наблюдений описывает данная модель и критерий Акаике (AIC), выбирающий наилучшию модель.

Выводы:
======================================
Оригинальный ряд и трендовая составляющая для обоих моделей не стационарны, т.к. стандартное отклонение и скользящее среднее зависят от времени. Тест Дики-Фуллера подтверждает данный вывод. 

Сезонная составляющая и остаток (для обоих моделей) стационарны, т.к. ряды являются константами, стандартное отклонение и скользящее среднее не зависят от времени. 

Лучший результат, который может дать R2 score - это 1.0. Если результаты больше или меньше 1.0 то, это говорит об очень большой неточности предсказания модели. Поскольку у наших моделей значения -289 и -1.9, что говорит о том, что они не точные.
Считается, что наилучшей будет модель с наименьшим значением критерия AIC, и в нашем случае это модель ARIMA с занчением 248.


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
* `NumPy` — библиотека с открытым исходным с такими возможностями, как поддержка многомерных массивов (включая матрицы), поддержка высокоуровневых математических функций, предназначенных для работы с многомерными массивами. 

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

Выбираем файл task2.ipynd и запускаем его. При этом файлы training.csv и testing.csv должны находиться в той же папке, что и task2.ipynd. 

Участники
======================
**Шуган Алена - 312 гр.**

1, 2 часть задания + визуализация решений с помощью библиотеки seaborn + соблюдение PEP8

**Сударева Валерия -  312 гр.**

3 часть задания
