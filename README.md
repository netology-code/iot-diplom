# Курсовая работа «Датчик движения для умного дома»

Курсовой проект представляет собой модуль управления системы "Умный дом" для жилой комнаты.

## 1 Основные решаемые задачи

+ 1.1 Включение/выключение основного света в комнате по нажатию на один из двух выключателей света (клавиша без фиксации);
+ 1.2 Включение ночной подсветки по сигналу от датчика движения (должна быть активна только в период времени с 23:00 до 7:00);
+ 1.3 Включение основного света по сигналу от датчика движения (должна быть активна только в период времени с 7:00 до 23:00);
+ 1.4 Будильник на заданное время с включением основного света и подачей питания на дополнительную нагрузку;
+ 1.5 Синхронизация внутренних часов от сервера времени.
+ 1.6 Отображение и настройка режимов работы с помощью жидкокристаллического индикатора и кнопок.

## 2 Структура модуля управления

В основе модуля управления должен лежать модуль ESP32, т.к. он обеспечивает беспроводное соединение по WiFi, что необходимо для соединения с сервером времени.
Управление светом и включение дополнительной нагрузки должно осуществляться с помощью электромагнитного реле.
Отсчет текущего времени можно реализовать как на ресурсах модуля ESP32, так и с помощью внешнего модуля RTС.
При включении питания следует осуществить попытку подключения к серверу времени и по его данным обновить текущее время модуля. Если связи с сервером времени нет, то следует начать отсчет времени со значения 00:00.
Можно использовать жидкокристаллический индикатор как с параллельной шиной подключения, так и с интефейсом I2C.
Для ввода команд можно использовать как дискретные кнопки, так и цифровую матричную клавиатуру.

## 3 Описание задач

### 3.1 Включение/выключение света в комнате по нажатию на один из двух выключателей света

Выключатели света представляют собой клавиши без фиксации. По каждому нажатию на любую из клавиш необходимо изменить состояние основного света. Основной свет подключается к выходу электромагнитного реле. Необходимо предусмотреть подавление дребезга контактов от клавиш.

### 3.2 Включение ночной подсветки по сигналу от датчика движения

Следует использовать датчик движения на основе пироприемника. Ночная подсветка подключается к выходу электромагнитного реле. Ночная подсветка может быть активна только в период времени с 23:00 до 7:00. В настройках модуля необходимо иметь возможность отключения ночной подсветки и задания времени активности ночной подсветки после срабатывания датчика движения.

### 3.3 Включение основного света по сигналу от датчика движения

Следует использовать датчик движения на основе пироприемника (можно использовать общий пироприемник от ночной подсветки). Основной свет подключается к выходу электромагнитного реле. В настройках модуля необходимо иметь возможность отключения управления основным светом по срабатыванию датчика движения и задания времени активности основного света после срабатывания датчика движения.

### 3.4 Будильник на заданное время с включением основного света и подачей питания на дополнительную нагрузку

В будильнике необходимо иметь возможность задавать время срабатывания, длительность подачи питания на дополнительную нагрузку, а также отключать будильник. Дополнительная нагрузка подключается к выходу электромагнитного реле. 

### 3.5 Синхронизация внутренних часов от сервера времени

Для синхронизации внутренних часов от сервера времени следует делать запрос по адресу: http://worldtimeapi.org/api/timezone/Europe/Moscow . Подключаться к сети следует по беспроводному соединению WiFi. В настройках модуля необходимо иметь возможность задания SSID и пароля сети, к которой предполагается подключение. Сеть по умолчанию: Wokwi-GUEST без пароля. После обновления данных о SSID и пароля сети следует делать попытку запроса к серверу времени.

### 3.6 Отображение и настройка режимов работы с помощью жидкокристаллического индикатора и кнопок

Модуль должен иметь возвожность задавать и отображать следующие настройки:
+ разрешение/запрет включения ночной подсветки по датчику движения;
+ время активности ночной подсветки после срабатывания датчика движения в диапазоне от 10 до 60 секунд; 
+ разрешение/запрет включения основного света по датчику движения;
+ время активности основного света после срабатывания датчика движения в диапазоне от 10 до 600 секунд; 
+ время срабатывания будильника;
+ разрешение/запрет будильника;
+ длительность подачи питания на дополнительную нагрузку при срабатывании будильника в диапазоне от 10 до 600 секунд;
+ SSID WiFi сети;
+ пароль WiFi сети;
+ ручной ввод текущего времени.

## Рекомендации по технической реализации

### Структура программы

Для каждой задачи создайте отдельную пару файлов .cpp и .h, что позволит структурировать Ваш проект.

### Как правильно задавать вопросы руководителю по курсовому проекту?

Что следует делать, чтобы все получилось: 
1. Попробовать найти ответ сначала самому в интернете. Ведь, именно это скилл поиска ответов пригодится тебе на первой работе. И только после этого спрашивать дипломного руководителя
2. В одном вопросе должна быть заложена одна проблема 
3. По возможности, прикреплять к вопросу скриншоты и стрелочкой показывать где не получается. Программу для этого можно скачать здесь https://app.prntscr.com/ru/
4. По возможности, задавать вопросы в комментариях к коду. 
5. Начинать работу над курсовым проектом как можно раньше! Чтобы было больше времени на правки. 
6. Делать курсовой проект по-частям, а не все сразу. Иначе, есть шанс, что нужно будет все переделывать :)  

Что следует делать, чтобы ничего не получилось: 
1. Писать вопросы вида “Ничего не работает. Не запускается. Всё сломалось.”
2. Откладывать курсовой проект на потом. 
3. Ждать ответ на свой вопрос моментально. Руководители по курсовому проекту - работающие разработчики, которые занимаются, кроме преподавания, своими проектами. Их время ограничено, поэтому постарайтесь задавать правильные вопросы, чтобы получать быстрые ответы. 
