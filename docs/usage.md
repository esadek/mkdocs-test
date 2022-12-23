# Usage

The `edges` model must be run enough times to match all edges (IDs). Five or six passes is usually sufficient. The `check_edges` model will show 0 when all edges have been matched. Edit your job commands for [dbt Cloud](https://docs.getdbt.com/docs/dbt-cloud/cloud-overview) or `run.sh` script for [dbt CLI](https://docs.getdbt.com/dbt-cli/cli-overview) to run the `edges` model however many times is necessary.

## dbt CLI

### Shell Script

Run the included `run.sh` shell script:

```bash
./run.sh
```

Additional intstrumentation can be created to evaluate the `check_edges` model to determine programatically whether to run the `edges` model subsequent times.

### Python Script

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
