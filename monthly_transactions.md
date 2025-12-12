<img width="228" height="176" alt="image" src="https://github.com/user-attachments/assets/c577dfdb-00fc-4d6e-9aa1-3c39019b33e1" />


- `id` is the primary key of this table.  
- The table contains information about incoming transactions.  
- The `state` column is an enum of type `["approved", "declined"]`.

**Task:**  
Write an SQL query to find, for each month and country:
- The total number of transactions  
- The total transaction amount  
- The number of approved transactions  
- The total amount of approved transactions  

Return the result table in any order.

**Example query (PostgreSQL dialect):**

```sql
SELECT
    TO_CHAR(trans_date, 'YYYY-MM') AS month,
    country,
    COUNT(*) AS trans_count,
    SUM(amount) AS trans_total_amount,
    COUNT(CASE WHEN state = 'approved' THEN 1 END) AS approved_count,
    SUM(CASE WHEN state = 'approved' THEN amount ELSE 0 END) AS approved_total_amount
FROM Transactions
GROUP BY TO_CHAR(trans_date, 'YYYY-MM'), country;

