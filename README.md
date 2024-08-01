# Introduction

In this project, I embarked on a journey to enhance my skills as an aspiring data analyst by utilizing SQL to analyze job-related data. The project involved working with multiple datasets to uncover insights related to job opportunities, required skills, and company information. By leveraging these datasets, I was able to derive valuable conclusions about the job market.

💡
SQL queries? Check them out here: [project_sql folder](/project_sql/)

# Background
Data hails from my [SQL Course](https://lukebarousse.com/sql). It's packed with insights on job titles, salaries, locations, and essential skills.

The project is centered around four key files that contain essential information:
- **Job Details:** This file includes job-related information such as job ID, job title, and job source.
- **Skills Details:** This file outlines skills with their corresponding skill names and IDs.
- **Job-Skill Mapping:** This file establishes a relationship between skill IDs and job IDs, indicating which skills are associated with which jobs.
- **Company Details:** This file contains information about companies, including company ID and company name.

By analyzing these datasets, I aimed to identify trends and insights such as the top-paying jobs, the most demanded skills, and the optimal skills for job seekers.

# Tools I Used 
For this project, I utilized the following tools:
- **SQL:** The backbone of my analysis, allowing me to query the database and unearth critical insights.
- **PostgreSQL:** A powerful relational database management system that allowed me to perform complex queries and analyses on the datasets.
- **Visual Studio Code (VS Code):** An integrated development environment (IDE) that I used for coding and managing my SQL scripts. I also explored various extensions to enhance my productivity and workflow.
- **Git & GitHub:** A platform for version control and collaboration, where I documented my project and shared my findings.
# The Analysis
Each query for this project aimed at investigating specific aspects of the data analyst job market.

Here's how I approached each question:

### 1.Top Paying Data Analyst Jobs

This query highlights the high paying opportunities in the field.
```sql
SELECT
    job_id,
    job_title,
    job_location,
    job_schedule_type,
    salary_year_avg,
    job_posted_date,
    name AS company_name

FROM
    job_postings_fact

LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id

WHERE
    job_title_short = 'Data Analyst' AND 
    job_location IN ('India', 'Anywhere') AND
    salary_year_avg IS NOT NULL

ORDER BY
    salary_year_avg DESC
    
LIMIT 10
```
![TopDataAnalystrole](https://github.com/user-attachments/assets/e5609fc6-ef64-4ad8-98f4-2de8b6ae3356)
This graph visualizing the salary for the top 10 data analysts.
### 2.Top Paying Job Skills
This query highlights the high paying skills in the field Data Analyst.
```sql
WITH top_paying_jobs AS(
    SELECT
        job_id,
        job_title,
        salary_year_avg,
        name AS company_name

    FROM
        job_postings_fact

    LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id

    WHERE
        job_title_short = 'Data Analyst' AND 
        job_location IN ('India', 'Anywhere') AND
        salary_year_avg IS NOT NULL

    ORDER BY
        salary_year_avg DESC
        
    LIMIT 10
)

SELECT
    top_paying_jobs.*,
    skills
    
FROM top_paying_jobs

INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id

ORDER BY
        salary_year_avg DESC
```
![Top Paying Skills for Data Analyst](assets\topskills.png)
This graph visualizing the top skills for data analyst.
### 3.Top Demanded Skills.
This query highlight the top 5 most demanded skills for data analyst.
```sql
SELECT
    skills,
    COUNT(job_postings_fact.job_id) AS demand_count
    
FROM job_postings_fact

INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id

WHERE
    job_title_short = 'Data Analyst'

GROUP BY
    skills

ORDER BY
    demand_count DESC

LIMIT 5;
```
| Skills      | Demand Count |
|-----------|------------|
| SQL       | 92,628     |
| Excel     | 67,031     |
| Python    | 57,326     |
| Tableau   | 46,554     |
| Power BI  | 39,468     |
### 4.Skills Based on Salary
Exploring the average salaries associated with different skills revealed which skills are the highest paying.
```sql

SELECT
    skills,
    ROUND(AVG(salary_year_avg), 2) AS avg_salary
    
FROM job_postings_fact

INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id

WHERE
   salary_year_avg IS NOT NULL AND job_title_short= 'Data Analyst' AND
   job_location IN ('India', 'Anywhere')

GROUP BY
    skills

ORDER BY
    avg_salary DESC

LIMIT 10;
```
| Skills       | Avg Salary  |
|--------------|-------------|
| PySpark      | 208,172.25  |
| Bitbucket    | 189,154.50  |
| Watson       | 160,515.00  |
| Couchbase    | 160,515.00  |
| DataRobot    | 155,485.50  |
| GitLab       | 154,500.00  |
| Swift        | 153,750.00  |
| Jupyter      | 152,776.50  |
| Pandas       | 151,821.33  |
| Elasticsearch| 145,000.00  |
### 5. Most Optimal Skills to Learn
Combining insights from demand and salary data, this query aimed to pinpoint skills that are both in high demand and have high salaries, offering a strategic focus for skill development.
```sql
SELECT 
    skills,
    COUNT(job_postings_fact.job_id) AS demand_count,
    ROUND(AVG(salary_year_avg), 2) AS avg_salary

FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    salary_year_avg IS NOT NULL AND job_title_short = 'Data Analyst'
    AND job_work_from_home = TRUE
GROUP BY
    skills
HAVING
    COUNT(job_postings_fact.job_id) > 10
ORDER BY
    demand_count DESC
LIMIT
    10;
```
| Skills       | Demand Count | Avg Salary   |
|--------------|--------------|--------------|
| SQL          | 398          | 97,237.16    |
| Excel        | 256          | 87,288.21    |
| Python       | 236          | 101,397.22   |
| Tableau      | 230          | 99,287.65    |
| R            | 148          | 100,498.77   |
| SAS          | 126          | 98,902.37    |
| Power BI     | 110          | 97,431.30    |
| PowerPoint   | 58           | 88,701.09    |
| Looker       | 49           | 103,795.30   |
| Word         | 48           | 82,576.04    |

# What I Learned 
Throughout this project, I gained a wealth of knowledge and experience, including:
- **SQL Proficiency:** I developed my SQL skills from basic queries to more advanced techniques, enabling me to manipulate and analyze data effectively.
- **Data Analysis:** I learned how to extract meaningful insights from raw data, which is crucial for making informed decisions in the field of data analytics.
- **Version Control:** I became familiar with GitHub and its functionalities, which are essential for managing projects and collaborating with others.
- **Tool Utilization:** I explored various features of VS Code and PostgreSQL, enhancing my coding efficiency and data management capabilities.
# Conclusions   
This project has been a significant stepping stone in my journey as a data analyst. By working with real-world datasets and applying SQL techniques, I was able to uncover valuable insights about the job market. The experience not only improved my technical skills but also deepened my understanding of data analysis processes. I look forward to applying what I have learned in future projects and continuing to grow in this field.

