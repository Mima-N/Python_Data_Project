# Overview

Welcome to my data analysis project focused on mapping the US job market for Data Analysts. This analysis was driven by a practical need to find optimal job oppourtunites by examining job posting data to determine the intersection of the highest-paying and most in-demand technical skills.

The data contains a detailed information on job titles, salaries, location and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salaries trends, and the sweet spot of demand and salary in data analytics.

# Questions

1. What are the most demanded skills for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What is the most optimal skill to learn for Data Analyst? (High Demand AND High Paying)

# Tools I Used
For this deep dive, I used several key tools:
- **Python:** The backbone of my analysis, allowing me to analyze the data and find critical insights using the following libraries:
    - **Pandas Library:** The library I used to analyze the data.
    - **Matplotlip Library:** The library I used to visualize data
    - **Seaborn Library:** The library I used to create more advanced visuals.

- **Jupyter Notebooks:** The tool I used to run my Python scripts which let me easily include my notes and analysis
- **Visual Studio Code:** For executing my Python scripts.
- **Git & GitHub:** For version control, sharing my work and project tracking.

# Data Preparation and Cleanup
To prepare the data for analysis, I started by importing and loading the dataset, followed by initial data cleaning tasks to ensure data quality.
```Python
# Importing Libraries
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data cleaning
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)

# Filter US jobs
df_US = dF[df['job_location'] == 'United States']
```

# The Analysis
Here's how I approached each question:
## 1. What are the most demanded skills for the top 3 most popular data roles?
To find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting.

View my notebook with detail steps here:      [2_Skill_Demand.ipynb](3_Project/2_Skill_Demand.ipynb)

### Visualize Data

```python
fig, ax = plt.subplots(len(job_titles), 1)


for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], legend=False, hue='skill_count', palette='dark:b_r')

plt.show()
```

### Results

![Visualization of Top Skills for Top Data Roles](/3_Project/images/skill_demand_top_data_roles.png)
*Bar graph visualizing the top skills for top data roles*

### Insights:

- **SQL** is the Universal Foundation: SQL is required by over half of all job postings in every single category, ranging from **51% (Data Analyst/Data Scientist)** to a high of **68% (Data Engineer)**.
- **Python** Dominates Advanced Roles: Python is the single most requested skill for both **Data Scientist (72%)** and **Data Engineer (65%)**, but it is less critical for **Data Analysts (27%)**.
- **Excel** is an Analyst Priority: Excel **(41%)** is the second most requested skill for the Data Analyst role, highlighting the importance of traditional business intelligence tools for that position.
- Overall, each role has a distinct technical focus: The **Data Analyst** prioritizes **Excel** and foundational reporting; the **Data Scientist** heavily requires **Python** and statistical languages like **R** for modeling; and the **Data Engineer** demands Cloud **(AWS, Azure)** and Big Data **(Spark)** infrastructure expertise.

## 2. How are in-demand skills trending for Data Analysts?

To find the trend of top in-demand skills, I aggregated skill counts monthly and re-analyzed the data based on percentage of total jobs instead of count. This query highlights the top 5 most in-demand job skills and their trends, showing which skills I should pay more attention to depending on their demand over these months.

View my notebook with detail steps here:  [3_Skills_Trend.ipynb](3_Project/3_Skills_Trend.ipynb)

### Visualize Data

```python

from matplotlib.ticker import PercentFormatter

df_plot = df_DA_US_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, palette='tab10', legend=False)

plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()
```

### Results

![Trending Top Skills for Data Analysts in the US](/3_Project/images/skill_trend_DA.png)
*Line graph visualizing the trendling top skills for data analysts in the US in 2025.*

### Insights:

- **SQL** consistently holds the highest demand (above 45%), but its requirements saw the largest volatility, dipping sharply in the second half of the year.
- **Tableau and Python** hovers in the mid-to-high 20% range and frequently cross paths, suggesting the market views them as equally important in the moderate skill tier for analysts.
- **Excel** usage requirements show a consistent, slow decrease throughout the year, falling from over 40% to near 35%.
- **SAS** is the least requested skill, consistently staying below 22% with minimal fluctuation.

## 3. How well do jobs and skills pay for Data Analysts?
To determine how well jobs and skills pay, I grouped the jobs and skills to determine median salary. This query also highlights the top paying skills and salaries of most in-demanded skills.
### Salary Analysis for Data Nerds

View my notebook with detail steps here: [4_Salary_Analysis.ipynb](3_Project/4_Salary_Analysis.ipynb)
#### Visualize Data
```python
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)
sns.set_theme(style='ticks')

ticks_x = (plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```
#### Results
![Salary Distribution of Data Jobs in the US](/3_Project/images/salary_boxplot.png)
*Box plot visualizing the salary distributions for the top 6 data job titles.*
#### Insights
- Median salaries are consistently higher for Senior roles, indicating clear financial progression with experience.
- **Data Scientists** and **Data Engineers** (both standard and senior levels) command the highest median salaries, all clustering around **$120K to $140K**, while Data Analysts sit slightly lower.
- The **Data Analyst** roles have the tightest interquartile ranges, suggesting less variability in salary for the middle 50% of people in those roles compared to the more spread-out distributions for Scientists and Engineers.
- **Data Scientists** and **Data Engineers** have outliers extending all the way to **$600K**, demonstrating high-value opportunities at the very top end of the market.

### Highest Paid and Most Demanded Skills for Data Analysts

#### Visualize Data
```python
fig, ax = plt.subplots(2, 1)

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, ax=ax[0], hue='median', palette='dark:b_r')

# Top 10 Most In-Demand Skills for Data Analysts
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, ax=ax[1], hue='median', palette='light:b')

plt.show()

```
### Results
Here's the breakdown of the highest paid and most in_demand skills for Data Analysts in the US:

![The Highest Paid & Most In-Demand Skills for Data Analysts in the US](/3_Project/images/highest_paid_and_most_in_demand_skills_for_data_analysts_in_the_usa.png)
*Two separate bar graphs visualizing the highest paid and most in-demand skill for data analysts in the US.*

### Insights:

- The top graph shows that maximum Data Analyst compensation is driven by rare, specialized infrastructure tools like `dplyr`, `bitbucket`, and `gitlab`. These skills command median salaries up to $200K, reflecting a 30-50% premium over common tools due to their scarcity.

- The second graph shows that the most in-demand foundational skills `Python`, `Tableau`, `SQL` dominate job postings with consistent median salaries (~$100K), demonstrating their importance for steady employability for data analysts.

- These reveals an inverse relationship between skill demand and pay. High-demand skills are competitively priced lower, while niche, emerging technologies offer the greatest return on investment. Maximizing earning potential means mastering the fundamentals and prioritizing expertise in specialized, low-supply tools.

## 4. What is the most optimal skill to learn for Data Analysts?

To find the most optimal skill to learn, I grouped the skills to determine median salary and likelihood of being in posting by calculating what percentage of all Data Analyst job postings require a specific skill. This query highlights the prevalence of certain technologies.

View my notebook with detail steps here:  [5_Skills_Optimal.ipynb](/3_Project/5_Optimal_Skills.ipynb)

### Visualize Data

```python
from adjustText import adjust_text
import matplotlib.pyplot as plt

plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])
plt.show()
```
### Results

![Most Optimal Skills For Data Analysts in the US](/3_Project/images/most_optimal_skills_for_data_analysts_in_the_usa_with_colouring_by_technology.png)
*A scatter plot visualizing the most optimal skills (high paying and high demand) for data analysts in the US.*

### Insights

- Programming skills (in blue) — Python and SQL stands out as the most valuable skills for data analysts, they feature prominently in both high median salaries over $90K and the largest percentage of job postings.
- Analyst tools such as "excel", "tableau", and "power bi" (orange dots) are in significant demand but tend to offer slightly lower median salaries compared to top programming and database technologies.
- Database (green) and cloud (red) technologies like "oracle" and "sql server" appear less frequently in job postings, but when required, they are associated with high median yearly salaries, highlighting their specialized value.

## What I Learned

This project not only provided critical market intelligence for the Data Analyst role but also served as a comprehensive training exercise, significantly advancing my proficiency in the Python data ecosystem. Here are a few specific things I learned:

- **Mastery of the Python Data Stack**: I deepened my technical expertise by using advanced Pandas techniques (e.g., vectorized operations like explode() and melt()), for complex data transformation. I also utilized Seaborn and Matplotlib to design insightful visualizations, demonstrating the ability to translate raw data into clear strategic narratives.
- **Emphasis on Data Integrity**: I learnt that thorough data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.
- **Strategic Skill Alignment**: The product emphasized the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary, and job availablity allows for more strategic career planning in the tech industry.

## Key Strategic Insights

The analysis of the job market for Data Analysts yielded three primary, actionable insights concerning skill valuation and career planning:

- **Premium on Specialization**: A direct correlation exists between complexity of a skill and its compensation. Advanced, specialized technologies such as Python and Oracle often lead higher median salaries compared to entry-level tools.

- **The Dynamic Skill Landscape**: Continuous shifts in required skills emphasize the necessity of ongoing professional development. Analysts who actively track these evolving trends and proactively acquire new high-value skills are best positioned for sustained career growth and relevance.

- **Maximizing Economic Returns**: Understanding which skills are both in-demand and well-compensated can guide data analysts in prioritizing learning to maximize their long-term earning potential and career mobility.

## Challenges I Faced
This project provided critical learning opportunities by forcing me to overcome several analytical hurdles:
- **Data Integrity**: Handling missing or inconsistent data entries requires careful consideration and robust data cleaning techniques to ensure the accuracy of the analysis.
- **Complex Visualization**: Conveying the multi-dimensional relationship between skill demand, median salary, and technology category was a significant design challenge. I addressed this by employing specialized plotting techniques, such as the strategic use of scatter plots with adjusted text labels (as seen in the optimal skills chart), to prevent overlap and present complex insights clearly and compellingly to the end-user.

- **Balancing Scope (Breadth vs. Depth)**: Deciding how deeply to dive into each analysis while maintaining a broad overview of the landscape required constant scoping to ensure comprehensive covergae without getting lost in details.

## Conclusion: Strategic Value and Future Vision
This comprehensive exploration into the US Data Analyst job market has yielded highly actionable insights, moving beyond simple data aggregation to provide a clear strategic roadmap for career growth. As the data landscape rapidly evolves, the importance of continuous skill adaptation cannot be understated. This project establishes a robust, repeatable foundation for ongoing market analysis, underscoring the necessity of continuous learning to maintain a competitive advantage in the field of data analytics.