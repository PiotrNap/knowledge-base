# Table View Fallback on Original Table's Security/Row-Level Policy

**Summary:**
Table views do not have their own RLS policies, so they fall back on the underlying table.

**Example**

```sql
CREATE TABLE invoices {
    id int
    company_id int
    amount numeric
};

ALTER TABLE invoices ENABLE ROW LEVEL SECURITY;

CREATE POLICY tenant_isolation
ON invoices
USING (company_id = current_setting("app.company_id")::int);

CREATE VIEW my_invoices AS
SELECT id, amount
FROM invoices;
```

If someone runs:

```sql
SELECT * FROM my_invoices
```

.. then they'll get results allowed to be accessed by the `invoices` table RLS policy.
Views do not get their own RLS policies or replace their original tables' policies.
