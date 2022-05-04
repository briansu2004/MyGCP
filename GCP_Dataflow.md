# GCP Dataflow

## Using OOTB built-in Dataflow template

Steps

### Get the Oracle table schema

### Create the dataset

### Create the table in the dataset

Use the same schema as the source table

```
CHART_ID:STRING,
ACCOUNT_NO:STRING,
CONVERSION_TYPE:STRING,
DESCRIPTION:STRING,
CONTROL_CLASS_ID:STRING,
ACCOUNT_TYPE:STRING,
POSTING_LEVEL:STRING,
DB_CR_IND:STRING,
REVALUATION:STRING,
I_C_FLAG:STRING,
DATE_CREATED:STRING,
USER_CREATED:STRING,
DATE_MODIFIED:STRING,
USER_MODIFIED:STRING,
USER1_VALUE:STRING,
SECURITY_STATUS:STRING,
XBRL_TAG:STRING,
RECONCILED:STRING,
REBILLABLE:STRING
```

Verify with a simple SQL script -

```
SELECT * FROM <project_id>.<dataset_id>.<table> LIMIT 10
```

### Create the GCS buckets

### Upload the JDBC driver to the GCS bucket

odjb10.jar works.

Other jar files like odjb8.jar should also work.

### Create the Dataflow job

We will use the built-in template “Jdbc to BigQuery”.

Item
Value

1
Job name
load\_<table>

2
Region endpoint
us-west1

3
Dataflow template
Jdbc to BigQuery

4
Jdbc connection URL string
jdbc:oracle:thin:@<ip>:<port>:<db_service>

5
Jdbc driver class name
oracle.jdbc.OracleDriver

6
Jdbc source SQL query
SELECT \* FROM <table>

7
BigQuery output table
<project_id>:<dataset_id>.<table>

8
GCS paths for Jdbc drivers
gs://<gcs_id>/ojdbc10.jar

9
Temporary directory for BigQuery loading process
gs://<gcs_id>\_dataflow-templates/temp

10
Temporary location
gs://<project_id>\_dataflow-templates/temp

11
Encryption
Google-managed encryption key

12
Jdbc connection username
<username>

13
Jdbc connection password
<encrypted>

14
Max workers
1

15
Number of workers
1

16
Worker region
Use job’s regional endpoint

17
Worker zone
Auto-select

18
Worked IP address Configuration
Private

19
Subnetwork
<Subnetwork>

### Run the dataflow job

### Verify the result

SELECT \* FROM <project_id>.<dataset_id>.<table> LIMIT 10
