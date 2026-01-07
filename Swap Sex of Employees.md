### Table: `Salary`

| Column Name | Type        |
|-------------|-------------|
| id          | int         |
| name        | varchar     |
| sex         | ENUM('m','f')|
| salary      | int         |

- `id` is the primary key.
- The `sex` column contains only `'m'` (male) or `'f'` (female).

---

### Problem

Write a **single `UPDATE` statement** (no `SELECT`, no temporary tables) that **swaps all `'f'` and `'m'` values** in the `sex` column:
- Change every `'m'` to `'f'`
- Change every `'f'` to `'m'`

---

### Example

**Input:**

| id | name | sex | salary |
|----|------|-----|--------|
| 1  | A    | m   | 2500   |
| 2  | B    | f   | 1500   |
| 3  | C    | m   | 5500   |
| 4  | D    | f   | 500    |

**Output:**

| id | name | sex | salary |
|----|------|-----|--------|
| 1  | A    | f   | 2500   |
| 2  | B    | m   | 1500   |
| 3  | C    | f   | 5500   |
| 4  | D    | m   | 500    |

---


```sql
UPDATE Salary
SET sex = CASE sex
            WHEN 'm' THEN 'f'
            ELSE 'm'
          END;
```

> **Why this works:**  
> - `CASE sex` compares the current value of the `sex` column.
> - If it’s `'m'`, set to `'f'`; otherwise (i.e., it’s `'f'`), set to `'m'`.
> - This is a single `UPDATE` statement with no intermediate steps — fully compliant with the requirements.

---

 
