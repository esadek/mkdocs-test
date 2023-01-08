# Usage

1. Run `queries`, `edges` and `check_edges`.
2. Run `edges` enough times to stitch all IDs. Query `check_edges` to determine if `edges` must be run again.
3. Run `id_graph`.

---

## dbt Core

The models can be run using a script.

### Shell Script

Run the included `run.sh` shell script:

```bash
./run.sh
```

The loop can be edited to run `edges` the necessary number of times.

```bash
#!/bin/sh

dbt run --full-refresh --select queries edges check_edges

for i in {1..5}
do
  dbt run --select edges
done

dbt run --select id_graph
```

### Python Script

A Python script can be used to automatically run `edges` to necessary number of times by evaluating the `check_edges` view to determine programatically whether to run the `edges` model subsequent times.

```python
from os import system
from sqlalchemy import create_engine


db = create_engine("dialect+driver://username:password@host:port/database")

with open("target/compiled/id_stitching/models/check_edges.sql") as file:
    query = file.read()

system("dbt run --full-refresh --select queries edges")

while db.execute(query).first()["count"]:
    system("dbt run --select id_graph")

system("dbt run id_graph")
```

## dbt Cloud

Create a job with the following commands:

```bash
dbt run --full-refresh --select queries edges
dbt run --select edges
dbt run --select edges
dbt run --select edges
dbt run --select edges
dbt run --select edges check_edges id_graph
```

Add or remove runs of `edges` using the `check_edges` view.
