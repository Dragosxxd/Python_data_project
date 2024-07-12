# Overview
In this project I intend to dive deep into the huge market of data jobs. The purpose of this project is to understand better what the top employers are asking for in regards to the skills for Data Analysts , Data Engineers and Data Scientist. This paper will be used as a guide to find the best fit for anyone who wishes to join this field.

The data sourced from [Luke Barousse's Python Course](https://www.youtube.com/watch?v=wUSDVGivd-8&t=36698s&ab_channel=LukeBarousse) which provides a foundation for my analysis, containing detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.


 # The Questions 

 Here are the questions I aim to answer in this project:

1. What are the skills most in demand for the top 3 most popular data roles?

2. How are in-demand skills trending for Data Analysts?

3. How well do jobs and skills pay for Data Analysts?

4. What are the optimal skills for data analysts to learn? (High Demand AND High Paying)


# Tools I Used
- **Python** : The backbone of my analysis, allowing me to analyze the data and find critical insights.I also used the following Python libraries:

  - **Pandas Library**: This was used to analyze the data.
  - **Matplotlib Library**: I visualized the data.
  - **Seaborn Library**: Helped me create more advanced visuals.
- **Jupyter Notebooks**: The tool I used to run my Python scripts which let me easily include my notes and analysis.
- **Visual Studio Code**: My go-to for executing my Python scripts.
- **Git & GitHub**: Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.

# Data Preparation and Cleanup
This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.
## Import & Clean Up Data
I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.

```python
# Importing Libraries
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```

# Filter Data jobs in Europe

``` python
european_countries = [
    "Albania", "Andorra", "Armenia", "Austria", "Azerbaijan", "Belarus", 
    "Belgium", "Bosnia and Herzegovina", "Bulgaria", "Croatia", "Cyprus", 
    "Czech Republic", "Denmark", "Estonia", "Finland", "France", "Georgia", 
    "Germany", "Greece", "Hungary", "Iceland", "Ireland", "Italy", "Kazakhstan", 
    "Kosovo", "Latvia", "Liechtenstein", "Lithuania", "Luxembourg", "Malta", 
    "Moldova", "Monaco", "Montenegro", "Netherlands", "North Macedonia", "Norway", 
    "Poland", "Portugal", "Romania", "Russia", "San Marino", "Serbia", "Slovakia", 
    "Slovenia", "Spain", "Sweden", "Switzerland", "Turkey", "Ukraine", "United Kingdom", 
    "Vatican City"
]

df_europe = df[df['job_country'].isin(european_countries)].copy()
df_skills = df_europe.explode('job_skills')
```

# The Analysis 
I have broken this presentation into 4 parts, one for each question that i want to adress. Here is how I approached each question:

# 1.What are the most demanded skills for the top 3 most popular data roles?

First I Wanted to look for the skills that appear the most among the top 3 Data Roles, and I filtered the dataset for exactly this purpose. This code presents the most popular Job Titles and their related top skills .

View my notebook with all the stepts i took here: [2_Skill_Demand.ipynb](3_Project/2_Skill_Demand.ipynb)

## Visualize Data 
```python
fig,ax = plt.subplots(len(job_titles) , 1,figsize=(7, 6 ))
sns.set_theme(style= 'ticks')
for i,job_title in enumerate(job_titles):
    df_plot = jobs_europe_perc[jobs_europe_perc['job_title_short'] == job_title].head(5)
    sns.barplot(data = df_plot , x='skills_percent',y='job_skills',ax = ax[i] , hue = 'skill_count',legend = False)
```
## Results 
![Likelyhood of skills required](Images\Skills_Demand.png)

# Insights
- As we can conclude from the graphs, the most versatile skill in this list is Python, being present in over *60%* of the Data Scientist jobs, in almost *60%* of the Data Enginner jobs, and in 1 of 3 data Analyst job
- On the second place we have sql, being equally as present as Python in the Data Engineer roles , the second most demanded skill for Data Scientist(*39%*) and the most demanded skill for Data Analysts(*45%*)
- For the Data Analyst roles the visualisation tools are the ones that are next in the top of most demanded skills(*Power Bi, Tableu*), and for the Data Scientist and Data Enginner more practical skill for managing databases are important(*Aws, Azure*)


# How are in-demand skills trending for Data Analysts?

For this question i tried to find some insights into how the demand for the top 5 skills was fluctuating during the year of 2023.

Here is the detailed notbook with my work: [3_Skill_Trend.ipynb](3_Project/3_Skill_Trend.ipynb)

# Visualise The Data
```python
from matplotlib.ticker import PercentFormatter
df_plot = DA_total_percent.iloc[:,:5]
sns.set_theme(style= 'ticks')
sns.lineplot(data=df_plot,dashes = False,legend = False , palette = 'dark:r')
sns.despine()
plt.title('Trending skills for Data Analysts',fontsize = 15)
plt.ylabel('Percent')
plt.xlabel('Month')
plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))
for i in range(5):
    plt.text(11.2, df_plot.iloc[-1, i], df_plot.columns[i], color='black')
```
## Results
![Skill_trend](Images\Skill_Trend.png)

# Insights

- We can see a slight decrease in the demand for SQL at the end of the year compared to the begining 
- From the graph we can observe a increase in the demand for Python over the year of 2023
- The other tools' demand had some fluctuation over the year but overall they remained steady
- The data on Europe Data Analyst jobs doesn't give us much information on how the demand for skills is going to change for the next year


# How much do the top 6 most popular jobs pay?

To indentify the Top 6 jobs I filtered for the jobs with the most job postings in Europe and ordered them through their median salary, and I tried to find out what is the pay gap between these jobs.

Here is the notebook with all my detailed work : [4_Salary_Analysis.ipynb](3_Project/4_Salary_Analysis.ipynb)

## Visualize the Data

```python
sns.boxplot(data = df_job_titles , x='salary_year_avg', y='job_title_short',order = df_order,palette = 'light:r')
sns.set_theme(style = 'ticks')
plt.xlim(0,270000)
plt.ylabel('Job Title')
plt.xlabel('Salary(USD)')
plt.title('Salary Distribution among the Top 6 Data Jobs in Europe')
ax.xaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y / 1000)}k'))
```

### Results 
![Salary](Images\Salary.png)
