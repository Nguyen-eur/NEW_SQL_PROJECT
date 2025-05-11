# Introduction
📊 Dive into the data job market! Focusing on data analyst / Business analyst, this project explores 💰 top-paying jobs, 🔥 in-demand skills, and 📈 where high demand meets high salary in data analytics.

🔍 SQL queries? Check them out here: [project_sql folder](/project_sql)
# Background
 This project was born from a desire to pinpoint top-paid and in-demand skills, in order to find optimal jobs. Coming from a Psychology major who are super interested with data and its power.
## The questions I wanted to answer through my SQL queries were:
1. What are the top-paying data analyst jobs?
2. What skills are required for these top-paying jobs?
3. What skills are most in demand for data analysts?
4. Which skills are associated with higher salaries?
5. What are the most optimal skills to learn?
# Tools I Used
For my deep dive into the data analyst job market, I harnessed the power of several key tools:

*  SQL: The backbone of my analysis, allowing me to query the database and unearth critical insights.
* PostgreSQL: The chosen database management system, ideal for handling the job posting data.
* Visual Studio Code: My go-to for database management and executing SQL queries.
* Git & GitHub: Essential for version control and sharing my SQL scripts and analysis, ensuring collaboration and project tracking.
# The Analysis
Each query for this project aimed at investigating specific aspects of the data analyst job market. Here’s how I approached each question:

## 1. Top Paying Data/Business Analyst Jobs
To identify the highest-paying roles, I filtered data analyst/business analyst positions by average yearly salary and location, focusing on The Netherlands. This query highlights the high paying opportunities in the field.
```sql
SELECT
    job_id,
    job_title,
    job_location,
    job_schedule_type,
    salary_year_avg,
    job_posted_date,
    name AS company_name
FROM job_postings_fact
LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
WHERE (job_title_short = 'Data Analyst' OR job_title_short = 'Business Analyst') 
AND (job_location LIKE '%Netherlands%'
   OR job_location LIKE '%,NL')
   AND salary_year_avg IS NOT NULL
ORDER BY salary_year_avg DESC
LIMIT 10
```
## Key Insights from the Data/Business Analyst Salary Analysis in the Netherlands
### 1. Salary Overview
The average salary for jobs with available salary data is quite **high**, but the dataset contains very few salary records (only 23 out of 5163 entries).
The **mean** salary is around €101,726 per year (based on available data).
Salaries range from around €53,014 (minimum) to €177,283 (maximum), indicating a large variation depending on role, experience, and employer.
### 2. Salary by Job Category
**Business Intelligence Analysts** tend to have slightly higher salaries than **Data Analysts** and **Business Analysts**.
Business Analysts are positioned **between** general Data Analysts and Business Intelligence Analysts in terms of salary.
### 3. Salary by Location
**Amsterdam** has the highest concentration of job postings and offers some of the highest salaries.
Some smaller cities (like Maassluis and Enkhuizen) report very high salaries, but these might be outliers due to fewer data points.
**The Hague** also has relatively competitive salaries.
### 4. Job Demand Trends
The majority of jobs (3159 out of 5163) are for Data Analysts, making it the most in-demand role.
Business Analysts (400) and Business Intelligence Analysts (112) have fewer postings, but they may offer better salary prospects.
## Meaningful Takeaways
Data Analyst roles dominate the market, making it the most accessible job, but salaries vary widely.
Location matters – Amsterdam, The Hague, and some niche cities offer better pay.
Business Intelligence roles tend to pay more, though there are fewer of them.
Many job listings don’t disclose salary information, making it difficult to fully assess pay trends.

![Top paying categories](image.png)

*Created by ChatGPT - Highest paid Job Category*

## 2. Skills for Top Paying Jobs
To understand what skills are required for the top-paying jobs, I joined the job postings with the skills data, providing insights into what employers value for high-compensation roles.
```sql
WITH top_paying_jobs AS (
    SELECT	
        job_id,
        job_title,
        salary_year_avg,
        name AS company_name
    FROM
        job_postings_fact
    LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
    WHERE
        (job_title_short = 'Data Analyst' OR job_title_short = 'Business Analyst') 
        job_location LIKE '%Netherlands%' 
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
    salary_year_avg DESC;
```
### Key Conclusions
#### 1. SQL is the Most In-Demand Skill:

SQL appears the most frequently, indicating that database management and querying skills are crucial for high-paying data and business analyst roles.
This aligns with industry trends where structured data handling is fundamental for analytics.
#### 2. Python is Highly Valued:

Python ranks second, confirming that coding and automation skills are becoming essential.
Python’s libraries (e.g., Pandas, NumPy, Scikit-Learn) make it a go-to tool for data analysis, machine learning, and automation.
#### 3. SAP is Relevant for Business Analysts:

The presence of SAP suggests that many high-paying roles require ERP system knowledge.
This is particularly relevant in finance, supply chain, and large enterprise operations.
#### 4. R is Still Used but Less than Python:

R appears multiple times, but Python is more prevalent.
This suggests that while R is still valuable for statistical analysis, Python has broader applications in analytics.
#### 5.Excel Remains a Core Skill:

Despite more advanced tools, Excel remains essential, especially for business reporting, financial modeling, and data visualization.
It shows that even in high-paying roles, fundamental spreadsheet skills are still needed.

## 3. In-Demand Skills for Data Analysts
This query helped identify the skills most frequently requested in job postings, directing focus to areas with high demand.
```sql
SELECT 
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst' 
    AND job_work_from_home = True 
GROUP BY
    skills
ORDER BY
    demand_count DESC
LIMIT 5;
```

### Key Takeaways

* **SQL** and **Excel** remain fundamental, emphasizing the need for strong foundational skills in data processing and spreadsheet manipulation.
* **Programming** and **Visualization** Tools like **Python**, **R**, and **Power BI** are essential, pointing towards the increasing importance of technical skills in data storytelling and decision support.

| Skill    | Demand Count |
|----------|-------------|
| SQL      | 2,005       |
| Python   | 1,574       |
| Excel    | 839         |
| Power BI | 765         |
| R        | 739         |

*Table of the demand for the top 5 skills in data analyst job postings*

## 4. Skills Based on Salary

Exploring the average salaries associated with different skills revealed which skills are the highest paying.

**The sample did not gave enough information to explore this TT**

## 5. Most Optimal Skills to Learn

By combining insights from **demand** and **salary data**, this query identifies skills that are both:
- **In high demand** (frequently required in job postings).
- **High-paying** (associated with above-average salaries).

This offers a **strategic focus** for skill development.

```sql
SELECT 
    skills_dim.skill_id,
    skills_dim.skills,
    COUNT(skills_job_dim.job_id) AS demand_count,
    ROUND(AVG(job_postings_fact.salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    (job_title_short = 'Data Analyst' OR job_title_short = 'Business Analyst')
    AND job_location = '%Netherlands%'
GROUP BY
    skills_dim.skill_id
HAVING
    COUNT(skills_job_dim.job_id) > 10
ORDER BY
    avg_salary DESC,
    demand_count DESC
LIMIT 25;
```
 Rank | Skill     | Demand Count |
|------|----------|-------------|
| 1    | SQL      | 2,325       |
| 2    | Python   | 1,779       |
| 3    | Excel    | 1,112       |
| 4    | Power BI | 922         |
| 5    | Tableau  | 835         |
| 6    | R        | 814         |
| 7    | Azure    | 529         |
| 8    | Word     | 405         |
| 9    | SAP      | 305         |
| 10   | Go       | 243         |
*Note: the query includes avg_salary, but the sample file did not include enough information for the Netherlands, this data is inconclusive*

## Key Conclusions from the Data

### 1. SQL and Python Lead in Demand for Data Analysts  
- **SQL (2,325 job postings)** and **Python (1,779 job postings)** are the most required skills.  
- This highlights the need for **database querying (SQL) and programming (Python)** in modern data analytics roles.

### 2. Business Intelligence (BI) and Reporting Skills Are Crucial  
- **Excel (1,112 job postings), Power BI (922), and Tableau (835)** are highly in demand.  
- These skills emphasize the **importance of data visualization and reporting** in analytical decision-making.

### 3. Cloud and Enterprise Tools Are Increasingly Relevant  
- **Azure (529 postings), SAP (305), and Go (243)** indicate a growing need for **cloud-based data analytics and enterprise solutions**.  
- Learning **cloud platforms (Azure) and ERP systems (SAP)** can provide a **competitive career advantage**.

---

🚀 **Takeaway:**  
**SQL, Python, and BI tools are must-have skills, while cloud computing and enterprise solutions offer additional career growth opportunities.**

# What I learned
### 🔥 **Key Skills Acquired**  
- 🧩 **Complex Query Crafting:**  
  - Mastered **advanced SQL techniques** for seamless **table merging**.  
  - Wielded `WITH` clauses like a pro for **efficient temp table maneuvers**.  

- 📊 **Data Aggregation Superpowers:**  
  - Got cozy with `GROUP BY` and turned **aggregate functions** (`COUNT()`, `AVG()`) into my data-summarizing sidekicks.  

- 💡 **Analytical Wizardry:**  
  - Leveled up my **real-world problem-solving skills**.  
  - Transformed **business questions into actionable SQL queries** packed with insights.  
# Conclusion
## 📊 Insights  

From the analysis, several key findings emerged:

### 💰 **Top-Paying Data Analyst Jobs**  
- The highest-paying data analyst role reaches an impressive **close to 400,000**!(according to the 23 data LOL)in The Netherlands and **650,000** in the US.

### 🛠 **Skills**  
- Advanced **SQL proficiency** is a common requirement for  data analyst roles, proving its critical importance.  

### 🔥 **Most In-Demand Skills**  
- **SQL dominates** as the most sought-after skill in the data analyst job market, making it an **essential** focus for job seekers.  

### 🎯 **What should I learn?**  
- Cloud-based data analytics is emerging? -> Learning **cloud platforms (Azure) and ERP systems (SAP)** can provide a **competitive career advantage**.

### 📈 **Optimal Skills for Maximizing Market Value**  
- **SQL leads both in demand and salary potential**, making it one of the best skills to learn for **career growth and job market competitiveness**. 

---

## 🔍 **Closing Thoughts**  

This project **enhanced my SQL skills** and provided valuable insights into the **data analyst job market**.  
- The findings act as my personal **guide for skill development and job search strategies**.  
- **Aspiring data analysts** can gain a **competitive edge** by focusing on **high-demand, high-salary skills**.  
- This exploration underscores the importance of **continuous learning** and staying ahead of **emerging trends** in any occupation.
- Thank you for reading.
