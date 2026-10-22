# Data Analyst Job Market Analysis with SQL

A SQL learning project exploring advertised salaries and skill demand in Data Analyst job postings. The project translates career-planning questions into PostgreSQL queries using joins, common table expressions, aggregation, and ranking.

## Decision context

The analysis is intended to help a job seeker investigate which skills appear frequently in remote Data Analyst postings and which are associated with higher advertised salaries. It provides exploratory evidence for learning priorities, rather than a prediction of hiring success or an individual's earnings.

## Questions and queries

| Question | Implementation | Population and output |
|---|---|---|
| Which remote postings advertise the highest annual salaries? | [Top-paying jobs](project_sql/1_top_paying_jobs.sql) | Data Analyst postings with location `Anywhere` and a non-null annual salary; top 10 postings |
| Which skills are listed in those postings? | [Skills in top-paying jobs](project_sql/2_top_paying_jobs_skills.sql) | Joins the top 10 postings to their recorded skills |
| Which skills appear most often? | [In-demand skills](project_sql/3_In_demand_skills.sql) | Data Analyst postings with location `Anywhere`; top five skills by associated posting count |
| Which skills have the highest average advertised salaries? | [Salary by skill](project_sql/4_top_paying_skills.sql) | Data Analyst postings marked both `Anywhere` and work-from-home, with annual salary data |
| Which skills combine salary information with a minimum posting count? | [Salary and demand shortlist](project_sql/5_most_optimal_skills.sql) | Work-from-home Data Analyst postings with annual salaries; skills appearing in more than 10 eligible postings, ordered by average salary then count |

The final query is a salary-first shortlist with a minimum sample threshold. Its filename uses "optimal", but the query does not establish an optimal learning strategy.

## Data model

| Table | Purpose | Key |
|---|---|---|
| `job_postings_fact` | Posting details, location, remote flag, salary fields, and posting date | `job_id` |
| `company_dim` | Company information | `company_id` |
| `skills_dim` | Skill names and categories | `skill_id` |
| `skills_job_dim` | Connects postings to skills | Composite `job_id, skill_id` |

One posting can have multiple skills. Skill-level counts therefore overlap and should not be added together as a count of unique jobs.

## SQL techniques demonstrated

- Joining posting, company, and skill tables.
- Using a CTE to select the top-paying postings before retrieving their skills.
- Calculating posting counts and average advertised salaries.
- Applying `WHERE` and `HAVING` filters to define eligible records and minimum counts.
- Sorting and limiting results to answer specific questions.
- Defining primary keys, foreign keys, and indexes.

## Repository contents

- [Analysis queries](project_sql/): the five numbered queries described above.
- [Database setup](sql_load/): database creation, schema, and CSV import scripts.
- `project_sql/query_1.sql`: a placeholder file, not part of the analysis.

## Reproducing the analysis

PostgreSQL and the matching four source CSV files are required.

1. Run [1_create_database.sql](sql_load/1_create_database.sql) from a connection permitted to create databases.
2. Connect to the new `sql_course` database.
3. Run [2_create_tables.sql](sql_load/2_create_tables.sql). Its ownership statements assume a `postgres` role; adapt them if your local roles differ.
4. Obtain `company_dim.csv`, `skills_dim.csv`, `job_postings_fact.csv`, and `skills_job_dim.csv`.
5. Adapt the local paths in [3_modify_tables.sql](sql_load/3_modify_tables.sql), or use its documented psql `\copy` alternative. Import company and skill records before postings, then import the posting-to-skill mappings.
6. Run the five numbered analysis queries against the populated database.

**Reproducibility status:** the source CSV files, dataset provenance, and saved query outputs are not included in this repository. The results cannot currently be independently reproduced from this repository alone.

## Interpretation and limitations

- **Results:** no numerical findings are asserted here because saved query outputs are not available in the repository.
- **Remote definition:** queries use different combinations of `job_location = 'Anywhere'` and `job_work_from_home = TRUE`. Their populations differ; align those definitions before comparing results.
- **Geography and time:** the analysis queries do not filter by country or posting date. They do not establish current Canadian or Saskatchewan hiring conditions.
- **Salary coverage:** salary calculations exclude postings without annual salary data. Missing salary information may bias the sample.
- **Currency and comparability:** currency, salary normalization, and source coverage need verification before interpreting averages across postings.
- **Demand:** counts measure skill associations in the available postings, not all market vacancies. Reposted or duplicated vacancies require separate checks.
- **Association:** higher average salaries for a skill do not show that learning it causes higher pay. Role seniority, employer, geography, and other skills may explain the difference.
- **Small samples:** the salary-by-skill query has no minimum count filter. Rare skills can rank highly on very few postings.
- **Ties:** top-10 selections have no explicit tie-breaker; equally paid postings at the cutoff may be selected inconsistently.

## Next improvements

1. Document the dataset source, license, extraction period, and any course or tutorial attribution.
2. Use one consistent definition of remote work across the queries.
3. Validate record counts, salary coverage, duplicate vacancies, and skill mapping integrity.
4. Export query outputs with sample counts and an explicit analysis date.
5. Write evidence-backed findings and explain how limitations affect any proposed learning priorities.

## Portfolio scope

This repository demonstrates SQL query construction and analytical reasoning in a learning context. It does not claim a client engagement, validated stakeholder requirements, or measured business impact.
