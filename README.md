# Introduction
This project offers a deep dive into the data analyst job market, highlighting top-paying roles, essential skills, and where high demand intersects with rewarding salaries in the field of data analytics.

SQL Queries ? Check them out here: [project_sql folder](/project_sql/)
# Background
Bascially I worked on this project actually understand the data job market and possible ways to leaverage by understanding the skillset/tools required.

### The questions I want to answer through my analysis:
1. What are the top paying data analyst jobs ?
2. What skills are required for this top paying jobs ?
3. What skills are most in demand for data analysts ?
4. Which skills are associated with higher salaries ?
# Tools I used
In working on this project, the following tools were highly effective in managing and creating this project:

1. **SQL**: This is the major input in why this project was created. It served as a strong foundation for writing th queries and perfoming the analysis.
2. **PostgreSQL**: This database management system was chosen for handling the data.
3. **Visual Studio Code**: To write queries effectively and effciently, this is a go-to platform. It's an effective workspace.
4. **Git & Github**: Very important for sharing SQL queries and also  version control. It's actually encourages collaboration among various teams.
# The Analysis
Each query for this project is aimed at investigating specific aspects of the job market. Here's how I approached each question:

### 1. Top Paying Data Analyst Job
To Identify the highest-paying roles, I filtered data analyst positions by average yearly salary and location, focusing on remote jobs. This queries highlights the high paying opportunities in the field.

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
LEFT JOIN
    company_dim ON company_dim.company_id = job_postings_fact.company_id
WHERE
    job_title_short = 'Data Analyst' AND
    job_location = 'Anywhere' AND
    salary_year_avg IS NOT NULL
ORDER BY
    salary_year_avg DESC
LIMIT
    10;
```
here’s a breakdown of some key insights:

**1. High-Paying Roles and Average Salaries**                                 
The highest-paying roles include titles such as Associate Director - Data Insights, indicating that management and leadership positions within data analytics command higher salaries (e.g., $255,829).
Senior roles or those with "Director" or "Lead" in the title generally reflect higher pay due to additional responsibilities and strategic decision-making.

**2. Common Skills Across Roles**             
SQL, Python, and Tableau appear frequently, highlighting their importance across many data analysis roles. These skills are foundational for querying databases, performing data manipulation, and creating data visualizations.
Tools related to cloud computing and big data (e.g., Azure, Snowflake, Databricks) are becoming increasingly important as companies continue to migrate data operations to cloud platforms.

**3. Specialized vs. Generalized Roles**      
Some roles focus on specific tools or technologies (e.g., Pandas, R, Snowflake), indicating a demand for specialized skills in certain areas.
Conversely, roles that include a variety of skills in the job requirements suggest that companies are looking for versatile analysts who can handle multiple aspects of data analytics, from database management to statistical analysis and machine learning.

**4. Industry Trends and Tools**              
There is a noticeable trend towards roles that integrate machine learning capabilities (with tools like Scikit-Learn or platforms like Watson), indicating that data analysts with experience in AI are in higher demand.
Data engineering skills are increasingly valuable, with tools like Airflow, Jenkins, and Kubernetes reflecting a blend of analytics and data pipeline development.

**5. Cross-Functional Roles**                
Skills such as Gitlab, Bitbucket, and Jupyter suggest that data analysts are expected to collaborate with software development teams and engage in cross-functional projects.
This implies that data analysts are taking on responsibilities traditionally associated with data engineering or even DevOps, expanding their roles beyond pure analysis.

![Top Paying Jobs](assets\top_10_paying_jobs_visual.png)
*Bar graph representing the top 10 paying jobs by their salary. Chatgpt helped me in creating the visuals*

### 2. Top Skillsets often required by employers
Different jobs have their different requirements; that is the skills necessary for the role. From this data, I have been able to identify the skills that employers look out for the most.
```sql
WITH top_paying_jobs AS (
    SELECT
        job_id,
        job_title,
        salary_year_avg,
        name AS company_name
    FROM 
        job_postings_fact
    LEFT JOIN
        company_dim ON company_dim.company_id = job_postings_fact.company_id
    WHERE
        job_title_short = 'Data Analyst' AND
        job_location = 'Anywhere' AND
        salary_year_avg IS NOT NULL
    ORDER BY
        salary_year_avg DESC
    LIMIT
        10
)

SELECT 
    top_paying_jobs.*,
    skills
FROM 
    top_paying_jobs
INNER JOIN skills_job_dim ON skills_job_dim.job_id = top_paying_jobs.job_id
INNER JOIN skills_dim ON skills_dim.skill_id = skills_job_dim.skill_id
ORDER BY
    salary_year_avg DESC
```


![Top 10 skills rated by employers](assets\top_10_skills_count.png)

### 3. Top skills needed for a data analyst job role.
People are often confused on what skills to learn when looking on take job roles as a data analyst. According to this 2023 data, we will will have an idea of which skills are most important for data analyst roles for employers

```sql
SELECT 
    skills,
    COUNT(skills_job_dim.skill_id) AS demand_count
FROM 
    job_postings_fact
INNER JOIN skills_job_dim ON skills_job_dim.job_id = job_postings_fact.job_id
INNER JOIN skills_dim ON skills_dim.skill_id = skills_job_dim.skill_id
WHERE
    job_title_short = 'Data Analyst' AND
    job_work_from_home = TRUE
GROUP BY
    skills
ORDER BY
    demand_count DESC
LIMIT 5
```
Here's a brief analysis:

- SQL is the most in-demand skill, with 7,291 mentions, suggesting its critical importance in managing and querying databases.

- Excel and Python follow closely, showing the need for a blend of traditional spreadsheet tools and programming languages.

- Tableau and Power BI, two popular data visualization tools, are also highly sought after, reflecting the industry's emphasis on visual analytics.

![Top Data Analyst skills](assets\top_data_analyst_skills_demand.png)

### 4. Top Paying Skillset
Which skillset pays the most ? Let's find out.

```sql
SELECT 
    skills,
    ROUND(AVG(salary_year_avg), 0) AS avg_salary
FROM 
    job_postings_fact
INNER JOIN skills_job_dim ON skills_job_dim.job_id = job_postings_fact.job_id
INNER JOIN skills_dim ON skills_dim.skill_id = skills_job_dim.skill_id
WHERE
    job_title_short = 'Data Analyst' 
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = TRUE
GROUP BY
    skills
ORDER BY
    avg_salary DESC
LIMIT 25
```
### Analysis:
- Pyspark is the highest-paying skill with an average salary of $208,172.

- Other high-paying skills include Bitbucket ($189,155), Couchbase and Watson (both at $160,515), and DataRobot ($155,486).

- These skills primarily relate to data processing, AI platforms, and cloud-based tools, which are in high demand in today's tech industry.

![Top Paying Skills](assets\top_paying_skills_visual.png)


# What I learned
1. I learned how to effectively use Git functions and Visual Studio Code to create the entire presentation.

2. SQL is a fundamental skill for every data analyst, as reflected in the data across various data roles.

3. I gained a deeper understanding of the data job market by analyzing the 2023 statistics.

4. I successfully joined and aggregated different tables using both INNER and LEFT JOINs.

5. I also created Common Table Expressions (CTEs) to simplify complex queries and improve readability.
6. I also created subqueries for filtering data, performing comparisons, and simplifying complex queries.

# Conclusions 
## Insights
Here are 4 key insights based on the whole analysis carried out:

- **High-Paying Roles**: Management and leadership roles, such as Associate Director - Data Insights, command the highest salaries (e.g., \$255,829), indicating that seniority and strategic decision-making significantly influence compensation.

- **Essential Skills**: SQL, Python, and Tableau are foundational across many data analysis roles, while cloud computing and big data tools like Azure, Snowflake, and Databricks are growing in importance as companies shift to cloud-based operations.

- **Specialized vs. Versatile Roles**: Specialized skills in tools like Pandas, R, and Snowflake are in demand, while some roles seek versatile analysts who can manage multiple aspects of data analytics, from database management to machine learning.

- **Industry Trends**: There is a rising demand for data analysts with machine learning and data engineering skills, as tools like Scikit-Learn, Airflow, and Kubernetes reflect the integration of AI and data pipeline development into analytics roles.
## Closing Thoughts
This project provided valuable hands-on experience with SQL, allowing me to perform operations such as filtering, joining data, creating subqueries, and working with CTEs. These tasks have strengthened my confidence in my SQL skills. For aspiring data analysts, it's essential to understand the specific skill sets required and align them with employer expectations in the job market.

Additionally, insights from this project emphasize the growing importance of cloud-based tools, data visualization, and machine learning, which can guide future skill development and career progression.
