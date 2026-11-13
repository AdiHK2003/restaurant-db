# Waiter Management Module (PL/SQL)

PL/SQL functions and procedures for managing waiters in a restaurant
database. Written by Adi Hari Krishna as part of [Restaurant Database Project/ IS420(PL/SQL)].

## Features

- `find_waiter_id(p_waiter_name)`: returns a waiter's ID (case-insensitive).
  Returns `NULL` if not found or if the name matches multiple waiters.
- `add_waiter(p_waiter_name, p_restaurant_name)`: adds a waiter to a
  restaurant looked up by name.
- `show_waiters_by_restaurant(p_restaurant_name)`: prints a restaurant's
  waiters in alphabetical order.

## Prerequisites

- Oracle Database with SQL*Plus, SQL Developer, or similar
- Tables: `restaurants`, `waiters (waiter_id, name, restaurant_id)`
- Sequence: `seq_waiter_id`
- Function: `find_restaurant_id` 
- `SET SERVEROUTPUT ON` to see output

## Setup

1. Create the tables, sequence, and `find_restaurant_id` (see `schema.sql`).
2. Run `member2.sql` to compile the code.

## Usage

```sql
SET SERVEROUTPUT ON;

EXEC add_waiter('Jane Doe', 'Hibachi Grill');
EXEC show_waiters_by_restaurant('Hibachi Grill');

DECLARE
    v_id INT;
BEGIN
    v_id := find_waiter_id('jane doe');
    DBMS_OUTPUT.PUT_LINE('ID: ' || v_id);
END;
/
```

## Tests

The test cases at the bottom of the file cover casing, whitespace,
over-long names, duplicate names, and empty restaurants. Some are meant to
expose limitations rather than pass:

| Test | Scenario | Expected |
|------|----------|----------|
| 1 | Restaurant name in different case | Works |
| 2 | Extra spaces around restaurant name | May fail (no trimming) |
| 3 | Very long waiter name | Inserts or hits column limit |
| 4 | Duplicate waiter at same restaurant | Allowed (no unique constraint) |
| ... | ... | ... |

## Known Limitations

- Names aren't trimmed, so leading/trailing spaces can cause lookups to fail.
- There's no uniqueness constraint, so waiters with the same names are allowed.
- `find_waiter_id` returns `NULL` when names are ambiguous, because it
  isn't scoped by restaurant.
- Errors are reported via `DBMS_OUTPUT`, not raised.

## License
[University of Maryland, Baltimore County]
