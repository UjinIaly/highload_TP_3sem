# 1. Тема и целевая аудитория
Twitter - американский сервис микроблогов и социальная сеть, в которой пользователи публикуют сообщения, известные как «твиты», и взаимодействуют с ними.

Аналоги:
- Threads
- Diaspora

## Целевая аудитория:
237.8 миллионов пользователей в день по всему миру.

Распределение пользователей по странам.

| Страна                            | Количество пользователей, млн |
|-----------------------------------|-------------------------------|
| США                               |             95.4              |
| Япония                            |             67.45             |
| Индия                             |             27.25             |
| Бразилия                          |             24.3              |
| Индонезия                         |             23                |
| Великобритания                    |             23.15             |
| Турция                            |             18.55             |
| Мексика                           |             17.2              |
| Саудовская Аравия                 |             15.5              |    
| Тайланд                           |             14.6              |

В месяц twitter посещает 330 миллионов пользователей.

## MVP
- Регистрация
- Авторизация
- Публикации
- Лайки
- Просмотр твитов

# 2. Расчет нагрузки
## Продуктовые метрики
- Количество уникальных пользователей за сутки(DAU) - 237.8 миллионов.  
- Количество уникальных пользователей за месяц(MAU) - 330 миллионов. 
- Количество уникальных пользователей за год - 353.9 миллионов.

### Авторизация

| Действие            |      Количество в день      |
|---------------------|-----------------------------|
| Авторизация         |           237.8 млн         |

Ежедневно в твиттер заходит 237.8 миллионов пользователей.

### Регистрация

| Действие            |  Среднее количество в день  |
|---------------------|-----------------------------|
| Регистрация         |            68.2к            |

Количество пользователей твиттера в 2022 году - 329 млн. В 2023 - 353.9 => за год зарегистрировалось 24.9 млн пользователей. То есть:
24.9 млн / 365 дней = 68.2 тыс/день

### Публикации

| Действие            | Среднее количество в минуту |
|---------------------|-----------------------------|
| Публикация твита    |             350к            |

Значит :
350000 * 60 * 24 = 504 млн твитов в день

### Лайки
| Действие            | Среднее количество в день на 1 пользователя|
|---------------------|--------------------------------------------|
| Лайк                |                   7.5                      |

Значит: 7.5 * 237.8 млн = 1.783 млрд лайков


### Просмотр ленты

| Действие            | Среднее количество в день на 1 пользователя|
|---------------------|--------------------------------------------|
| просмотр ленты      |                    75                      |

Пользователи заходят в твиттер 5 раз в день, при обновлении ленты загружается 30 публикаций, значит если пользователь будет смотреть публикации просто подряд 1 раз зайдя в твиттер, то мы получим минимальное число запросов : 3, посчитаем минимальное количество запросов для всех: 3 * 237.8 = 713.4 млн запросов,
Если мы посчитаем, что пользователь зашёл 5 раз и прочитал 75 твитов таким образом, то получим:
5 * 237,8 =  1 189 млн запросов. Возьмём среднее значение : 951.2 млн запросов.

## Технические метрики
### Хранилище

Средняя длина твита поставляет 67.9 символов, хотя часто пользователи делят очень большое сообщение на несколько твитов, но мы будем считать каждую публикацию отдельно. Максимальный размер изображения, прикладываемого в твите - 5 МБ, берём среднее значение - 2.5 МБ. Из всех твитов только 10 процентов содержать картинки, из этого може посчитать:

2.5МБ * 10% + 120байт/1 048 576 ~ 0.25 МБ

К сожалению твиттер не ведёт статистику по суммарному количеству твитов за всё время, но если примерно посчитать: с 2006 по 2009 было отправлено 5 млрд твитов, а дальше в среднем пользователи отправляли в среднем по 500 млн твитов в среднем до конца 2023 года, а значит примерный размер хранилища для публикаций:
5 млрд * 0,25 + 0.25 МБ * 504 млн * 60 * 24 ~ 640 ПБ.

В профиле пользователя 2 картинки(аватарка и фон), если учесть ещё текст и его публикации, хранилище под публикации мы посчитали выше, значит будем учитывать только текст, фон и аватарку: аватарка - не больше 2 МБ,
фон - не больше 5 МБ, текст весит мало => получаем среднее значения веса профиля ~ 3.5МБ

Всего в твиттере: 1.3 млрд аккаунтов, значит:

1.3 млрд * 3.5 МБ ~ 4.2 ПБ нужно для хранения всех профилей пользователей.

### Расширение хранилища
За 2023 год было опубликован 21 млрд твитов, значит за год сгенерировалось на 0.25 МБ * 504 млн * 60 * 24 ~ 45.75 ПБ контента. В среднем каждый год с 2009 публикуется более 500 млн твитов в день, значит ежегодный прирост: 45.75 ПБ.

### RPS

Авторизация:
[237.8 миллионов уникальных пользователей в сутки * 1 раз в сутки] / 86400 сек ~ 2752.3 RPS

Регистрация:
[24.9 миллионов новых пользователей в год * 1 раз] / 86400 сек / 365 дней ~ 0.8 RPS

Публикация:
[350 тысяч новых публикаций в минуту * 1 раз] / 60 сек ~ 5833.3 RPS

Лайки:
[237.8 млн пользователей в день * 7.5 раз] / 86400 ~ 20642.4 RPS

Просмотр ленты:
[237.8 млн пользователей в день * 5 запросов в день] / 86400 ~ 13761.6 RPS

Итого получается: 42990.4 RPS

### Сетевой трафик

Посчитаем нагрузку при публикации твитов и при просмотре.

При публикации твитов:

(0.25МБ * 350к) / 60 ~ 1483.3 МБ/с ~ 1.4 ГБ/с

При просмотре публикаций:

(0.25МБ * 237.8 млн * 5 * 30) / 24 /60 /60 ~ 103211.8 МБ/с ~ 103.2 ГБ/с

Итого суммарно: 103.2 ГБ/с

# 3. Глобальная балансировка нагрузки


### 3. Логическая схема
![Схема Базы данных](/images/bd.png)

### 4. Физическая схема


Твитер использует:
 - Hadoop
 - Manhattan
 - FlockDB
 - MetricsDB
 - BlobStore
 - Redis
 - Memcache

 Поговорим о каждой по-отдельности

### Hadoop

Hadoop для анализа социальных сетей, рекомендаций, тенденций, аналитики API, прогнозирования вовлеченности пользователей, таргетинга рекламы, рекламной аналитики, обработки показов твитов, создания резервных копий MySQL, резервных копий Manhattan и хранения интерфейсных журналов scribe.
Twitter управляет одним из крупнейших кластеров Hadoop в мире.

Файловая система Hadoop хранит более 500 петабайт данных, работающих в десятках тысяч экземпляров. Управление всем кластером осуществляется с помощью функции федерации Hadoop, выполняющей сотни тысяч ежедневных заданий Hadoop и десятки миллионов ежедневных задач Hadoop. Для этого более 150 тыс. сервисов и 130 млн контейнеров работают ежедневно.

### Manhattan

![Схема Manhattan](/images/Manhattan.png)

Манхэттен разделен на четыре уровня: интерфейсы, службы хранения, механизмы хранения и ядро с конечной моделью согласованности.

Высокая доступность предпочтительнее строгой согласованности. Для обеспечения высокой доступности Manhattan широко используется репликация.

Механизм хранения данных Manhattan был разработан собственными силами с возможностью подключения, чтобы при необходимости подключать любые внешние механизмы хранения данных в будущем.

### FlockDB

FlockDB - это распределенное хранилище данных graph, созданное для быстрого обхода графика, хранения списков смежности, поддержки высокой скорости операций добавления, удаления и обновления, разбиения на миллионы записей, горизонтального масштабирования и выполнения запросов обхода графика.

Twitter использовал ее для хранения социального графика с такой информацией, как "кто за кем следит" и тому подобное.

### MetricsDB

База данных Metrics используется в Twitter для хранения показателей. Скорость обработки показателей составляет более 5 миллиардов показателей в минуту при 25 тысячах запросов в минуту.

Metrics DB с использует алгоритм сжатия, встроенной в Facebook базы данных временных рядов, Gorilla.

База данных Metrics обеспечивает поддержку нескольких зон, разделение показателей и эффективность сжатия по сравнению с другими хранилищами данных, используемыми в Twitter. Используя алгоритм сжатия Gorilla, Twitter сократил использование пространства на 95%.

### BlobStore

Blobstore - это масштабируемая система хранения данных Twitter для хранения пользовательских изображений, видео и других больших двоичных объектов. Это позволило Twitter сократить расходы, связанные с хранением загруженных пользователями изображений с твитами.

Это высокопроизводительная система, способная обслуживать изображения за считанные десятки миллисекунд при пропускной способности в сотни тысяч запросов в секунду.

Когда изображение загружается в Blobstore, оно синхронизирует изображение во всех центрах обработки данных Twitter с помощью сервера асинхронной очереди.

### Redis и Memcache

Благодаря реализованному кэшированию Twitter предоставляет клиентам около 120 ГБ данных в секунду.

Созданный Twitter Twemcache пользовательская версия Memcache, подходящая для крупномасштабного производственного развертывания.

Платформа имеет сотни экземпляров кэша с более чем 20 ТБ данных в памяти из более чем 30 сервисов. В целом уровень кэширования обслуживает более 2 триллионов запросов в любой обычный день.

Кэширование в Twitter имеет два основных варианта использования:

Первая : данные хранятся в памяти, чтобы избежать попаданий в базу данных. 
Вторая : используется как буфер памяти для хранения элементов, которые дорого вычислять снова и снова.

Twemproxy, облегченный прокси-сервер для протокола Memcache и Redis, позволяет масштабировать уровень кэширования по горизонтали, сводя к минимуму подключения к внутренним серверам кэширования.

Внутри Twitter запущено множество служб кэширования, и одна из них работает на Redis. Кластеры Redis кэшируют прямые сообщения пользователей, расходы на рекламу, показы и вовлеченность.

### 5. Технологии

| Технология  | Область применения  | Мотивация |
| -------|:----------:|:-------:|
| Redis | Database | Высокая надежность, большое комьюнити, open-source, простота |
| Amazon DynamoDB | Database | Высокая надежность, большое комьюнити |
| ClickHouse | Database | Высокая надежность, хорошо подходит для решения задачи |
| Amazon S3 | Database | Большое комьюнити, CDN |
| Memcached | Database | Высокая надежность, большое комьюнити |
| Typescript, React | Frontend | Статистическая типизация, большое коммьюнити |
| Golang | Backend | Высокая производительность, легко работать с асинхронностью, можно использовать в микросервисах |
| QT, C++ | Desktop | Стабильность, простота, высокая производительность |
| Nginx | Balancer | Большое коммьюнити, высокая скорость работы |
| Kafka | Message broker | Большое коммьюнити, высокая скорость работы |

### 6. Схема проекта
![4](https://github.com/EuphoriaAbsorber/Steam/assets/65418582/1564121f-4a82-488d-8eb3-f0b64094dce9)

### 7. Список серверов
### Хранилище

1. Рассмотрим FTPS-сервера, которые раздают игровые файлы с HDD дисков.
В разделе "Сетевой трафик" была оценка на пиковый трафик общей загрузки пользователей в течение суток - 19.1 ТБ/с. Нужно взять по крайней мере столько серверов, чтобы суммарная пропускная способность у них была больше.  Так как такие сервера передают большие файлы, то будет использоваться протокол FTPS.
В разделе "Технические метрики" также была оценка хранилища, необходимого для хранения хотя бы одной копии игры ~ 840 ТБ. С учётом того, что надо файлы каждой игры хранить хотя бы в двух экземплярах(бекап), то можно умножить это число на 2. Итого получим 1780 ТБ. В [официальной статистике Steam](https://store.steampowered.com/stats/content/) можно увидеть, что суммарный трафик на загрузку в регионах Северная Америка, Европа и Азия составляет посчти полный трафик по миру, а также трафик по этим регионам сравним. Следовательно, сервера с раздачей файлов нужно иметь во всех этих регионах. CDN в данном случае не поможет, так как раздавать нужно большие файлы и от кеширования смысла почти нет. Итого по регионам нужно получить такое покрытие по характеристикам(в скобках трафик и хранение данных): Северная Америка(6ТБ/с, 1 780 ТБ), Европа(6ТБ/с, 1 780 ТБ), Азия(9ТБ/с, 1 780 ТБ). Остальные регионы могут скачивать файлы с серверов главных трёх регионов. На сервер надо поставить побольше дисков, иначе производительность сервера упрется в скорость чтения с малого количества дисков - для SSD для одного диска это может быть до 1 ГБ/с (кусочки больших файлов будут считываться последовательно. Нет причин полагать, что будут считываться кучочки по 1 KB. Также на сервере будет много ядер процессора. [Исследование скорости](https://www.researchgate.net/figure/NVMe-SSD-multithread-reading-throughput-for-4-kilobytes-blocks-and-up-to-256-threads_fig28_349520331)). Если на сервер поставить 10 таких дисков, то суммарная передача данных может достичь 10 ГБ/с. Сетевые карты можно взять с пропускной способностью 16 ГБ/с с учетом дальшейго возможного ускорения. Тогда для покрытия 6 ТБ/с понадобится 6 * 1024 / 16 = 614 серверов(возьмем х1.2 для запаса - 737). Для покрытия 9 ТБ/с понадобится 9 * 1024 / 16 = 922 серверов(возьмем х1.2 для запаса - 1 106). SSD можно выбрать по 1 TB, тогда для хранения понадобится 178 серверов(с учётом распределения 10 дисков на 1 сервер), что меньше уже взятого количества серверов. Количество оперативной памяти для сервера не очень важно. 
Сборка сервера:

|RAM|CPU|Network|Disk|
|------|------------------------------|------|--------|
|4x32GB|2xE5-2680 v4                  |10GB/s|10xSSD1T|
|      |14 ядер, 28 потоков, 3.30 GHz |      |        |

2. Сервера, на которых хранятся шарды из [раздела 4](https://github.com/EuphoriaAbsorber/Steam/blob/main/README.md#4-%D1%84%D0%B8%D0%B7%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B0%D1%8F-%D1%81%D1%85%D0%B5%D0%BC%D0%B0). На одном сервере могут храниться данные 50 млн пользователей, внутри одного сервера эти таблицы также разбиты на таблицы по 2 млн пользователей для ускорения работы. Итого для покрытия всех пользователей нужно 5 серверов. С учетом того, что схема репликации 1 Master + 2 Slaves получаем 15 серверов всего. Для сервера важны оперативная память и процессор, данных не так много.

Сборка сервера:

|RAM|CPU|Network|Disk|
|------|------------------------------|------|--------|
|8x32GB|2xE5-2680 v4                  |10GB/s|2xSSD1T|
|      |14 ядер, 28 потоков, 3.30 GHz |      |        |

### Backend-сервера
1. Сервер с микросервисом авторизации и in-memory базой Redis - нужно много памяти. Таких серверов будет 2, второй на случай падения первого.
 
 Сборка сервера:

|RAM|CPU|Network|Disk|
|------|------------------------------|------|----------|
|8x32GB|2xE5-2680 v4                  |10GB/s|2xSSD512GB|
|      |14 ядер, 28 потоков, 3.30 GHz |      |          |

2. Основные backend-сервера. На одном сервере будет основная DynamoDB, ещё 3 аналогичных сервера для репликации. С двух серверов Slaves будет производиться чтение, на Master - запись, и один для бекапа. В [разделе 2](https://github.com/EuphoriaAbsorber/Steam/blob/main/README.md#rps) пиковый rps был оценен в ~ 24000, с чем эти сервера справятся. Для серверов нужна оперативная память для работы Redis, в которой хранятся чаты. Из [раздела 2](https://github.com/EuphoriaAbsorber/Steam/blob/main/README.md#rps) в сутки сервисом пользуются 70 млн человек. Там же была оценка, что один пользователь отправляет за сутки 10 сообщений длиной по 20 символом. Посчитаем, сколько нужно памяти для хранения. 70 млн  * 10 * 20  байт = 14 000 000 000 байт ~ 13 ГБ. Возьмем с запасом х2, будет 26 ГБ. В оперативную память нормально поместится, так как можно считать, что раз в сутки сообщения очищаются. 

Сборка сервера:

|RAM|CPU|Network|Disk|
|------|------------------------------|------|----------|
|8x64GB|2x6258R                       |10GB/s|2xSSD512GB|
|      |28 ядер, 56 потоков, 4 GHz    |      |          |

3. Сервера для сбора статистики в ClickHouse. Нужно сделать упор на размер дисковых хранилищ, а также на количество ядер в процессорах. Таких будет 3 сервера.

Сборка сервера:

|RAM|CPU|Network|Disk|
|------|------------------------------|------|----------|
|4x64GB|2x6258R                       |10GB/s|10xSSD1T  |
|      |28 ядер, 56 потоков, 4 GHz    |      |          |

### Балансировщики
В последних версиях 1 воркер nginx может обработать 1024 подключений. Учитывая, что 1 воркер работает в одном потоке, нужно взять процессор получше.

Сборка сервера:

|RAM|CPU|Network|Disk|
|------|------------------------------|------|-----------|
|8x64GB|2x6258R                       |10GB/s| 2xSSD512GB|
|      |28 ядер, 56 потоков, 4 GHz    |      |           |

Такой сервер может обработать 2 * 56 * 1024 = 114 688 подключений. Из раздела 2: пиковый суточный онлайн - 32 миллиона. Большинство(можно оценить процентов в 60) просто находятся в игре в момент пикового онлайна, никаких запросов к серверу не совершается. Тогда в худшем случае понадобится 0.4 * 32 000 000 / 114 688 ~ 112 серверов-балансировщиков. Возьмем с запасом - получим 150.