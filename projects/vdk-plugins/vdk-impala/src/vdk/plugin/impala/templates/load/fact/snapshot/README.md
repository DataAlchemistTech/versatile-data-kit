### Purpose:

This template can be used to load raw data from Data Lake to target 'Snapshot Periodic Fact Table' in Data Warehouse.
In summary, it appends a snapshot of records observed between time t1 and t2 from the source table to the target table,
truncating all present target table records observed after t1.

### Details:

<https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/periodic-snapshot-fact-table/>

### Template Name (template_name):

- "periodic_snapshot"

### Template Parameters (template_args):

- target_schema   - SC Data Warehouse schema, where target data is loaded
- target_table    - SC Data Warehouse table of DW type 'Snapshot Periodic Fact Table', where target data is loaded
- source_schema   - SC Data Lake schema, where source raw data is loaded from
- source_view     - SC Data Lake view, where source raw data is loaded from
- last_arrival_ts - Timestamp column, on which increments to target_table are done
- check           - (Optional) Callback function responsible for checking the quality of the data. Takes in a table name as a parameter which will be used for data validation
- staging_schema  - (Optional) Schema where the checks will be executed. If not provided target_schema will be used as default

### Prerequisites:

In order to use this template you need to ensure the following:
- {source_schema}.{source_view} exists
- {target_schema}.{target_table} exists
- {source_schema}.{source_view} has the exact same schema as {target_schema}.{target_table}
- {last_arrival_ts} is timestamp column suitable for 'Snapshot Periodic Fact Table' increments

### Sample Usage:

Say there is SDDC-related 'Snapshot Periodic Fact Table' called 'fact_sddc_daily' in 'history' schema.
Updating it with the latest raw data from a Data Lake (from source view called 'vw_fact_sddc_daily' in 'default' schema) is done in the following manner:

```python
def run(job_input):
    # . . .
    template_args = {
        'source_schema': 'default',
        'source_view': 'vw_fact_sddc_daily',
        'target_schema': 'history',
        'target_table': 'fact_sddc_daily',
        'last_arrival_ts': 'updated_at',
    }
    job_input.execute_template('load/fact/snapshot', template_args)
    # . . .
```

### Example

See full example of how to use the template in [our example documentation](https://github.com/vmware/versatile-data-kit/wiki/SQL-Data-Processing-templates-examples#append-strategy-periodic-snapshot-fact).
