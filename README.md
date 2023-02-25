## Hello, I'm Ahmed 

### Who am I ?

I'm a Data / Analytics engineer who is excited about engineering and how we can apply best practises from software engineering to Big Data.

Here are some ideas to get you started:

- 🔭 I'm now using Python and SQL to create data solutions, data pipelines, and data monitoring tooling.

- 🌱 Currently, I'm studying Big Data Engineering Technology ( Airflow, Spark, Kafka, Kubernetes,...etc)

- 👯 I'm searching for opportunities to work on data engineering and analytics engineering projects.

- 📫 How to reach me: via email pevolution.ahmed@gmail.com

**Languages**:

- Python

- Java

- Javascript

- SQL

- Bash Scripting

**Data related tools**:

- DBT (data build tool)

- BigQuery datawarehouse (Column-based Database)

- Github actions as CI/CD tool

- Data visualization using( metabase, Google data studio, Microsoft Power BI, Amplitude)

- Jupyter notebook

- Apache Airflow as a workflow orchestration tool

- Microsoft Power BI
- Cribl for Real-time Observability Pipelines
- Apache Spark
- Greate Expectation as a data validation tool

- MySQL , Mongodb (Relational and Documnet based databases)

**Technical knowledge I have**:

- Statistics and Propability

- Software engineering

- Data Warehouse Modeling (Dimintional - ERD)

- Data Analysis

- Machine learning using scikit-learn

- Deep learning using pytorch and tensorflow

I use dbt on a daily basis to create (staging, basic, dimensional, and fact) models, with DAG (Directed Acyclic Graph) for auditing.

In addition, in the production environment, I typically utilise pre-built packages such as (dbt utils, dbt expectations,... etc).

**Coding Snippets**:
  
```
{% macro  extract_most_common_properties_from_mobile(
   event_name,
   start_date=dbt_date.n_days_ago(90),
   end_date=dbt_date.today(),
   extra_filter="and 1=1"
)
%}

with first_plat_props as (
   select 
      location_id as country_id,
      JSON_EXTRACT_SCALAR(event_properties, '$.Name') as event_name,
      count (distinct user_id) as number_users
   from 
      {{ source('ios', 'events') }} 
   where 
      event_type = {{ event_name}}
      and e_id is not null
      and date(start_time) between {{ start_date } and {{ end_date }}
      {{ extra_filter }}
   {{ dbt_utils.group_by(n=2) }}
),
second_plat_props as (
  select 
    e_id as event_id,
    JSON_EXTRACT_SCALAR(event_properties, '$.Name') as event_name,
    count (distinct user_id) as number_users
  from 
    {{ source('android', 'events') }} 
  where 
    event_type = {{ event_name}}
    and e_id is not null
    and date(start_time) between {{ start_date } and {{ end_date }}
    {{ extra_filter }}
  {{ dbt_utils.group_by(n=2) }}
),
all_names as (
  select * from first_plat_props
  union all 
  select * from second_plat_props
),
all_names_sum as (
   select 
       event_id,
       event_name,
       sum(number_users) as number_users
    from
       all_names
    {{ dbt_utils.group_by(n=2) }}
),
name_counts as(
   select 
      event_id,
      event_name, 
      row_number() over (partition by event_id order by number_users desc) as rownumber
   from 
      all_names_sum
)
select
   event_id,
   event_name
from 
   name_counts
where 
   rownumber = 1
    
{% endmacro %}

```
