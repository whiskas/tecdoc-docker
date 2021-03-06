# tecdoc-docker

Работаю над очередным проектом содержащим каталог для поиска запчастей. Бизнес прикупил мне дампы с базами для работы. Как всегда, ставлю импортировать дампы в MySQL. Процесс импорта долгий, жуть. Даже если вы обзавелись [SSD M2](https://ru.wikipedia.org/wiki/M.2), процессором от интел поядряней, 64 ГБ ОЗУ и больше, эта петрушка в среднем занимает часов 5-6 минимум. Базы я, обычно, для таких целей разворачиваю с помощью docker и docker-compose. Операционка для таких задач - Linux.

Последовательность действий у меня в среднем такая

- Разворачиваю mysql 5.7 и postgres

```
docker-compose up -d
```

- Запускаю import csv или sql файлов (это шаг долгий, чёрт возьми очень долгий)
- Проверяю что в MySql всё загрузилось
- Запускаю импорт из MySQL в postgres (тут тоже, можно в отпуск сходить)

```
pgloader mysql://tecdoc:tecdoc@localhost/tecdoc postgresql://tecdoc:tecdoc@localhost/tecdoc
```

- В результате получаются два docker volume которые можно запаковать в zip и тащить в prod. Prod сервера на docker swarm или kubernetes

В основном для SQL СУБД стараюсь использовать Postgres. Дабы облегчить жизнь, себе будущему, выкладываю конфиги docker-compose для разных SQL СУБД.

Порывшись в сети вижу, что можно получить td1q2018 из открытых источников. Сделал docker volumes для

- [mysql-5.7-td1q2018-docker-volume.zip](https://mega.nz/#!1VAUlI5J!z8HzJlKM-XgNxZnTTSih0l03yb5vsLFZf-rt12-RXlU)
- [mysql-8.0-td1q2018-docker-volume.zip](https://mega.nz/#!sdRmmQpJ!vs7MTiBratWsXDVC3e8VW_nV5wx_nPdwUCVM8FV-N0Q)
- [pg-11-td1q2018-docker-volumes.zip](https://mega.nz/#!wYxUCKpJ!QTlJDIY4aTDarzwN2OYPy4Qwcx3rCjiiqaV9ehWjpIw)

Краткая нисрукция для начинающих

- качаем с помощью клиента mega.nz
- распаковываем
- берём, к примеру, docker-compose-pg-11.yml переименовываем в docker-compose.yml
- запускаем

```
docker-compose ud -d
```

Пользуемся. Если что то не получается пишем [issues](https://github.com/overpod/tecdoc-docker/issues) к этому репозиторию, чем смогу помогу.

## Уважаемые изготовители дампов для SQL

Я тут выше описал способ улучшить потребительские свойства вашего продукта.
Предлагаю распостранять такие вот базы в виде docker-volumes
Это сильно упростит жизнь, мне и таким же как и я.

Планирую выкладывать приложение GRAPHQL для PG-TECDOC
и примеры использования его для Front приложений.
