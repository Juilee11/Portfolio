# SQL Validation — Sample Queries

Illustrative example of how I validate backend/database data against UI test cases, using a typical e-commerce schema (users, orders, order_items — modeled after the kind of flow tested in [test-cases-testrail.md](./test-cases-testrail.md)).

## Sample schema (for reference)

```sql
-- users(id, email, first_name, last_name, created_at)
-- orders(id, user_id, status, total_amount, created_at)
-- order_items(id, order_id, product_id, quantity, unit_price)
-- products(id, name, price, sku)
```

## 1. Verify order total matches sum of line items (data consistency check)

Used to confirm the total shown in the UI ("order overview" step in checkout) matches what's actually stored — catches bugs where the UI total drifts from backend calculation.

```sql
SELECT
    o.id AS order_id,
    o.total_amount AS ui_total,
    SUM(oi.quantity * oi.unit_price) AS calculated_total,
    (o.total_amount - SUM(oi.quantity * oi.unit_price)) AS diff
FROM orders o
JOIN order_items oi ON oi.order_id = o.id
GROUP BY o.id, o.total_amount
HAVING o.total_amount <> SUM(oi.quantity * oi.unit_price);
```
*Any rows returned here indicate a bug — orders where the UI/stored total doesn't match the sum of line items.*

## 2. Find orphaned order_items (referential integrity check)

Confirms every order_item points to a valid, still-existing order — a common source of "ghost" cart items or reporting bugs after deletes.

```sql
SELECT oi.*
FROM order_items oi
LEFT JOIN orders o ON o.id = oi.order_id
WHERE o.id IS NULL;
```

## 3. Check for duplicate SKUs (data quality check)

```sql
SELECT sku, COUNT(*) AS occurrences
FROM products
GROUP BY sku
HAVING COUNT(*) > 1;
```

## 4. Validate a specific regression scenario: user with 0 orders should not appear in "recent purchasers" report

Example of validating a backend query used to power a specific feature/report against expected test data.

```sql
SELECT u.id, u.email
FROM users u
LEFT JOIN orders o ON o.user_id = u.id
WHERE o.id IS NULL
  AND u.id IN (
      SELECT user_id FROM recent_purchasers_report
  );
```
*Expected result: 0 rows. Any rows returned means the report logic is incorrectly including users with no orders.*

## 5. Join example — orders with customer name for a manual spot-check

```sql
SELECT
    o.id AS order_id,
    u.first_name,
    u.last_name,
    o.status,
    o.total_amount
FROM orders o
INNER JOIN users u ON u.id = o.user_id
WHERE o.created_at >= CURRENT_DATE - INTERVAL '7 days'
ORDER BY o.created_at DESC;
```
