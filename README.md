# Домашнее задание к занятию "Типы и структура СУБД" - Иванов Игорь

## Задача 1

Архитектор ПО решил проконсультироваться у вас, какой тип БД 
лучше выбрать для хранения определённых данных.

Он вам предоставил следующие типы сущностей, которые нужно будет хранить в БД:

- электронные чеки в json-виде,
- склады и автомобильные дороги для логистической компании,
- генеалогические деревья,
- кэш идентификаторов клиентов с ограниченным временем жизни для движка аутенфикации,
- отношения клиент-покупка для интернет-магазина.

Выберите подходящие типы СУБД для каждой сущности и объясните свой выбор.

---

Электронные чеки в json-виде. 
Документоориентированная СУБД, например, MongoDB. JSON хорошо поддерживается в MongoDB, и она предоставляет богатые возможности для работы с документами.

Склады и автомобильные дороги для логистической компании. 
Реляционная СУБД, например, PostgreSQL или MySQL. Реляционные СУБД хорошо подходят для организации структурированных данных.

Генеалогические деревья.
Графовая СУБД, например, Neo4j. Графовые СУБД предоставляют эффективный способ моделирования и обработки связей между сущностями.

Кэш идентификаторов клиентов с ограниченным временем жизни для движка аутентификации.
In-memory СУБД, такие как Redis или Memcached. Эти СУБД эффективно хранят данные в памяти и обеспечивают высокую производительность для кэширования.

Отношения клиент-покупка для интернет-магазина.
Реляционная СУБД, такая как PostgreSQL или MySQL. Реляционные СУБД обеспечивают возможность моделирования связей между таблицами, что удобно для хранения информации о покупках клиентов.

## Задача 2

Вы создали распределённое высоконагруженное приложение и хотите классифицировать его согласно 
CAP-теореме. Какой классификации по CAP-теореме соответствует ваша система, если 
(каждый пункт — это отдельная реализация вашей системы и для каждого пункта нужно привести классификацию):

- данные записываются на все узлы с задержкой до часа (асинхронная запись);
- при сетевых сбоях система может разделиться на 2 раздельных кластера;
- система может не прислать корректный ответ или сбросить соединение.

Согласно PACELC-теореме как бы вы классифицировали эти реализации?

---

Данные записываются на все узлы с задержкой до часа (асинхронная запись):

CAP-классификация: CP (Consistency and Partition tolerance).
PACELC-классификация: PA/EL (Partition tolerance and Availability / Eventual consistency and Latency).

При сетевых сбоях система может разделиться на 2 раздельных кластера:
CAP-классификация: AP (Availability and Partition tolerance).
PACELC-классификация: PA/EL (Partition tolerance and Availability / Eventual consistency and Latency).

Система может не прислать корректный ответ или сбросить соединение:
CAP-классификация: AP (Availability and Partition tolerance).
PACELC-классификация: PA/EL (Partition tolerance and Availability / Eventual consistency and Latency).

## Задача 3

Могут ли в одной системе сочетаться принципы BASE и ACID? Почему?

---

Принципы BASE и ACID являются противоположными по своей природе и подходу к обеспечению надежности системы.
ACID-принципы стремятся обеспечить согласованность данных, гарантируя атомарность, целостность, изолированность и долговечность транзакций. В этом случае данные считаются достоверными и согласованными в любой момент времени.
BASE-принципы, с другой стороны, фокусируются на обеспечении доступности, возможности работать в режиме мягкого состояния и конечной согласованности. Это означает, что система может оставаться доступной и функционировать, даже если она находится в мягком, не полностью согласованном состоянии на определенном этапе.
Таким образом, в одной системе не могут сочетаться одновременно принципы BASE и ACID, поскольку они преследуют разные цели и основные аспекты обеспечения надежности системы. Выбор принципов будет зависеть от конкретных требований и приоритетов системы.

## Задача 4

Вам дали задачу написать системное решение, основой которого бы послужили:

- фиксация некоторых значений с временем жизни,
- реакция на истечение таймаута.

Вы слышали о key-value-хранилище, которое имеет механизм Pub/Sub. 
Что это за система? Какие минусы выбора этой системы?

---

Key-value-хранилище с механизмом Pub/Sub - это система, которая предоставляет возможность хранить данные в виде пар ключ-значение и подписываться на определенные события, которые происходят в хранилище. Например Redis.

Механизм Pub/Sub позволяет публиковать сообщения в каналы и подписываться на эти каналы для получения уведомлений о новых сообщениях. Таким образом, при изменении значения ключа или при истечении таймаута можно публиковать соответствующие сообщения, на которые другие компоненты системы смогут подписаться и принять соответствующие действия.

Минусы:

- сложность масштабирования: Если необходимо обрабатывать большой поток сообщений или поддерживать множество подписчиков, могут возникнуть проблемы с производительностью и масштабируемостью системы.

- надежность: Если система Pub/Sub выходит из строя, это может привести к потере сообщений или невозможности получения уведомлений о событиях. Это особенно критично для данных с ограниченным временем жизни и необходимостью реагировать на истечение таймаутов.

- сложность управления: Поддержание и конфигурирование системы Pub/Sub требует определенных знаний и усилий, чтобы обеспечить ее правильную работу и поддержку.

- ограничения использования: Система Pub/Sub может быть полезна для определенных случаев использования, но может оказаться неэффективной или излишней для других задач. Поэтому важно обратить внимание на требования системы и правильно выбрать подходящую систему хранения данных.
