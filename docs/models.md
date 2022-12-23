# Models

The primary ouput of this package is `id_graph`. There are a few intermediate models used to create this model.

![Models Diagram](assets/models.svg)

## queries

Generates select statements which pull IDs from your tables.

| sql_to_run |
| --- |
| SELECT DISTINCT email::TEXT AS node_a, 'email' AS node_a_label, user_id::TEXT AS node_b, 'user_id' AS node_b_label FROM db.web.identifies WHERE COALESCE(email::TEXT, '') != '' AND COALESCE(user_id::TEXT, '') != '' |
| SELECT DISTINCT phone::TEXT AS node_a, 'phone' AS node_a_label, user_id::TEXT AS node_b, 'user_id' AS node_b_label FROM db.app.identifies WHERE COALESCE(phone::TEXT, '') != '' AND COALESCE(user_id::TEXT, '') != '' |

## edges

Combines the results of those select statement to create a table containing edges (IDs) the first time it is run, and matches edges on subsequent runs.

| rudder_id | original_rudder_id | node_a | node_a_label | node_b | node_b_label | timestamp |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | 1 | user@company.com | email | 123456 | user_id | 2022-12-21 02:48:41.406655 |
| 1 | 2 | 555-555-5555 | phone | 123456 | user_id | 2022-12-21 02:48:41.406655 |

## check_edges

Determines if there are still edges to match.

| rows_to_update | consolidation_needed |
| --- | --- |
| 0 | FALSE |

## id_graph

Creates an ID graph table.

| rudder_id | node | label | latest_timestamp |
| --- | --- | --- | --- |
| 1 | user@company.com | email | 2022-12-21 02:48:41.406655 |
| 1 | 123456 | user_id | 2022-12-21 02:48:41.406655 |
| 1 | 555-555-5555 | phone | 2022-12-21 02:48:41.406655 |