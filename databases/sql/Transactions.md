# Транзакции
Транзакция - это последовательность операций над данными имеющая начало и конец

Транзакция это последовательное выполнение операций чтения и записи. Окончанием транзакции может быть либо сохранение изменений (фиксация, commit) либо отмена изменений (откат, rollback). Применительно к БД транзакция это нескольких запросов, которые трактуются как единый запрос.

***Транзакции должны удовлетворять свойствам ACID.***

## ACID

### Атомарность (Atomicity)
Транзакция либо выполняется полностью либо не выполняется вовсе.

### Согласованность (Consistency)
При завершении транзакции не должны быть нарушены ограничения накладываемые на данные (например constraints в БД). Согласованность подразумевает, что система будет переведена из одного корректного состояния в другое корректное.

### Изолированность (Isolation)
Параллельно выполняемые транзакции не должны влиять друг на друга, например менять данные которые использует другая транзакция. Результат выполнения параллельных транзакций должен быть таким, как если бы транзакции выполнялись последовательно.

### Устойчивость (Durabillity)
После фиксации изменения не должны быть утеряны.


## Журнал транзакций
Журнал хранит изменения выполненные транзакциями, обеспечивает `атомарность` и `устойчивость` данных в случае сбоя системы.

Журнал содержит значения, которые данные имели до и после их изменения транзакцией. `Write-ahead log strategy` обязывает добавлять в журнал запись о предыдущих значениях до начала, а о конечных после завершения транзакции. В случае внезапной остановки системы БД читает лог в обратном порядке и отменяет изменения сделанные транзакциями. Встретив прерванную транзакцию БД выполняет ее и вносит изменения о ней в журнал. Находясь в состоянии на момент сбоя, БД читает лог в прямом порядке и возвращает изменения сделанные транзакциями. Таким образом сохраняется устойчивость транзакций которые уже были зафиксированы и атомарность прерванной транзакции.

Простое повторное выполнение ошибочных транзакций недостаточно для восстановления.

### Пример
На счету у пользователя 500$ и пользователь решает снять их через банкомат. Выполняются две транзакции. Первая читает значение баланса и если на балансе достаточно средств выдает деньги пользователю. Вторая вычитает из баланса нужную сумму. Допустим, произошел сбой системы и первая операция не выполнилась, а вторая выполнилась. В этом случае мы не можем повторно выдать деньги пользователю без возврата системы в изначальное состояние с положительным балансом.
