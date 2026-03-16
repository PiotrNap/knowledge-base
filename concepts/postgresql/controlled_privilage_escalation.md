## Controlled Privilage Escalation Technique

**Summary**:
You use SECURITY DEFINER to control the function-invocation privilages. By default, all functions run with the SECURITY INVOKER privileges (the caller).

When you mark a function with

```sql
CREATE FUNCTION ...
SECURITY DEFINER
```

... it executes the function with the **function owner**, not the caller.

**Example Use Case:**
Imagine you have a table _payments_, and only app_admin can SELECT from it. You'd want you app_user to be able to check whether a specific payment exists in the table. You can do this by running a function with the app_admin privileges.

```sql
CREATE FUNCTION payment_exists(pid uuid)
RETURNS boolean
SECURITY DEFINER
LANGUAGE plpgsql
AS $$
BEGIN
    RETURN EXISTS (
        SELECT 1 from payments WHERE id = (pid)
    );
END;
$$;
```

and ...

```sql
GRANT EXECUTE ON FUNCTION payment_exists(uuid) TO app_user;
```

will make sure that app_user can call the `payment_exists` function without running SELECT on payments table.
