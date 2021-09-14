# Индексы
Индексы позволяют уменьшит количество последовательных просмотров табицы (seq scan), что позволяет сильно увеличить скорость выполнения запросов.

## Построение индекса
1. `create index`
```sql
create index <идентификатор индекса> on <имя таблицы> (<[поле1, поле2, ...]>);
```
Эта операция блокирует всю таблицу, что в свою очередь препятствует любым изменениям в этой таблице.

2. `create index concurrently` - не блокирует таблицу, но и строится дольше, чем `create index`. Однако PostgreSQL не гарантирует, что эта операция выполнится успешно. В случае конфликтов индекс будет помечен как недействительный и не будет принимать участие в запросах.

## Типы индексов
Чтобы посмотреть, какие типы индексов доступны, нужно выполнить запрос `select * from pg_am`. (в случае с PostgreSQL)
- B-Tree
- Hash
- GIST
- GIN
- SP-GIST
- BRIN

***В большинстве случаев `b-tree` и `hash` индексов будет достаточно.***

### ***b-tree***
Является типом по умолчанию в PostgreSQL и как показывает практика, в большинстве случаев b-tree будет достаточно.

В PostgreSQL используется b-tree Лемана-Яо. Что это за дерево такое? Дерево Лемана-Яо позволяет выполнять значительное количество операций чтения и записи над одним и тем же индексом.

B-tree хранит в себе данные в отсоритрованном виде, что дает нам возможность быстрого поиска нужного элемента. Еще одним немаловажным фактором является то, что b-tree в PostgreSQL можно читать как в прямом, так и в обратном порядке.

К примеру такой запрос `select min(id), max(id) from test` есть ни что иное как простое чтение первого и последнего элемента в дереве. Это очень быстрая операция.

### ***hash***
Этот тип индексов необходим, если используется тип данных, которых `нельзя разумно отсортировать`, или если нужно понять `содержит ли таблица такое значение или нет`.

Хеш-индексы можно представлять как табличку, где впервой колонке хеш от значения (где хеш это результат работы некоторой функции, которая преобразует переданное ей на вход значение в строку фиксированного размера), а вторая колонка самое значение.

- хеш-индексы не подходят для сортировки, потому как хеш (по факту это строка) нельзя разумно отсортировать;
- поиск по частичному ключу не поддерживается;
- доступны только операторы `=`, `<=>`, `IN()`.

## Минусы индексов
- Индексы занимают место на диске и в памяти
- Индексы замедляют запись в таблицу, так как базе данных нужно поддерживать их при каждом обновлении данных