

Дана таблица `Activity`, где каждая запись отражает активность игрока в определённую дату:

| player_id | device_id | event_date | games_played |
|-----------|-----------|------------|--------------|

- Первичный ключ: `(player_id, event_date)`.
- Нужно найти **долю игроков**, которые **зашли в игру на следующий день после своей первой даты входа**.
- Результат — одно число `fraction`, округлённое до **2 знаков после запятой**.

Формула:  
```
fraction = (число игроков с активностью на first_login + 1 день) / (общее число игроков)
```

---

### Решение (PostgreSQL)

```sql
SELECT ROUND(
    COUNT(CASE WHEN next_day.player_id IS NOT NULL THEN 1 END) * 1.0 / COUNT(*),
    2
) AS fraction
FROM (
    SELECT player_id, MIN(event_date) AS first_login
    FROM Activity
    GROUP BY player_id
) AS first_visits
LEFT JOIN Activity AS next_day
    ON first_visits.player_id = next_day.player_id
    AND next_day.event_date = first_visits.first_login + INTERVAL '1 day';
```

> **Для MySQL** замените строку с датой на:  
> ```sql
> AND next_day.event_date = DATE_ADD(first_visits.first_login, INTERVAL 1 DAY)
> ```

---

### Кратко — как это работает

1. **Подзапрос `first_visits`** находит первую дату входа для каждого игрока (`MIN(event_date)`).
2. **LEFT JOIN** с таблицей `Activity` ищет запись на **следующий день** (`first_login + 1`).
3. **`COUNT(CASE ...)`** считает, у скольких игроков такая запись существует.
4. **Деление на `COUNT(*)`** даёт долю от общего числа игроков.
5. **`ROUND(..., 2)`** — округление до двух знаков.


```
