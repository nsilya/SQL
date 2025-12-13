```markdown
### Описание задачи

Даны две таблицы:

- **`Signups`**: содержит всех пользователей (`user_id`, `time_stamp`).
- **`Confirmations`**: содержит попытки подтверждения (`user_id`, `time_stamp`, `action`), где `action` — либо `'confirmed'`, либо `'timeout'`.

Требуется для **каждого пользователя** из `Signups` вычислить **confirmation rate** — долю подтверждённых попыток среди всех запрошенных:

- Если пользователь не запрашивал подтверждение — его rate = `0.00`.
- Результат округлить до **двух знаков после запятой**.
- Вывести **всех пользователей**, даже без подтверждений.

Ожидаемый формат вывода:
```
| user_id | confirmation_rate |
```

---

### Решение

```sql
SELECT 
    s.user_id,
    ROUND(AVG(CASE WHEN c.action = 'confirmed' THEN 1.0 ELSE 0.0 END), 2) AS confirmation_rate
FROM Signups s
LEFT JOIN Confirmations c ON s.user_id = c.user_id
GROUP BY s.user_id;
```

---

### Кратко

- Используем `LEFT JOIN`, чтобы включить всех пользователей.
- Преобразуем `'confirmed'` в `1.0`, остальное — в `0.0`.
- Считаем среднее (`AVG`) → получаем долю подтверждений.
- Группируем **только по `user_id`**.
- Алиас колонки — **точно `confirmation_rate`** (snake_case, без кавычек).
```
