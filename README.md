Finding Anomalies in Curriculum Access
===

![](logo.png)

### Table of Contents
---

I.   [Project Overview](#i-overview)
1.   [Description     ](#1-description)
2.   [Deliverables    ](#2-deliverables)

II.  [Project Summary ](#ii-summary)
1.   [Goals           ](#1-goals)
2.   [Findings        ](#2-findings)
3.   [Recommendations ](#3-recommendations)

III. [Data Context    ](#iii-data-context)
1.   [Data Dictionary ](#data-dictionary)

IV.  [Reproduction    ](#iv-reproduction)

<br>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![](divider.png)

<br>

### I. Overview
---

#### 1. Description

In preparation of the upcoming board meeting, this report was created to answer several key questions posed by management with regard to access of Codeup curricula for web development and data science programs.

#### 2. Deliverables

- Jupyter [Notebook](https://nbviewer.jupyter.org/github/anomaly-work-alone/project_anomaly_detection_curriculum/blob/main/anomaly_report.ipynb) containing anomaly report of curriculum access
    - Answers at least five of seven questions asked
- Google [Slide]() in the form of an executive summary
- Emailed links of deliverables to management

### II. Summary
---

#### 1. Goals

There are several questions asked of the analyst team to find anomalies in user access in specific avenues of exploration:

- Which lesson appears to attract the most traffic consistently across cohorts (per program)?
- Are there students who, when active, hardly access the curriculum? If so, what information do you have about these students?
- Is there any suspicious activity, such as users/machines/etc accessing the curriculum who shouldn’t be? Does it appear that any web-scraping is happening? Are there any suspicious IP addresses?
- At some point in 2019, the ability for students and alumni to access both curricula should have been shut off. Do you see any evidence of that happening? Did it happen before?
- What topics are grads continuing to reference after graduation and into their jobs (for each program)?

The team set out to answer these questions utilizing an access log `.txt` file in conjunction with the cohorts table available on the Codeup SQL `curriculum_logs` database.

#### 2. Findings

In pursuit of answering these questions, the team discovered several lessons which were most utilized across the cohorts for each program, such as Classification and MySQL overviews for the data science program, which was found consistent with students post graduation access as well. Student activity during their active window, defined as the curriculum access between the start and end dates for their respective cohort, revealed 26 students with less than 1% of the most active user. When exploring the data for access that presented as suspicious in terms of traffic volume, several users were found with substantial single day traffic that was much greater than five times the IQR above the third quartile. Finally, when considering if there was a change in traffic following the termination of inter-program curriculum access, the findings were inconclusive due to insufficient data from the year prior. Evidence of possible changes in traffic trends was found in early January which could be further explored.

#### 3. Recommendations

These findings can be further explored with the insights gained in this initial venture. Clarification with admin responsible for this data can further alleviate existing mystery surrounding the results. For brevity in answering the specific questions asked of the analysis team for this report, the findings within are found to be adequate in presenting as is, and should this data need to be revisited at a later time, this report may serve as a launchpad for further query. In the interest of protecting Codeup assets and curriculum integrity, it is a more immediate recommendation to identify the users found with exorbitant traffic volume to determine any potential compromise that may have occurred and consult with the legal department as appropriate.

### III. Data Context
---

This data is acquired from a combination of the most recent access log `.txt` file, supplemented with user cohort data from the `cohorts` table in the curriculum_logs SQL database. For access to this information, please reach out to Codeup curriculum development. It is prepared by converting all date variables to the appropriate datetime data type and dropping columns unnecessary to answer the questions asked.

#### Data Dictionary

Upon wrangling of the log file and SQL data, the DataFrames used in this report contain observations with the values defined in the table below.

| Variable    | Definition                          | Data Type |
|:-----------:|:-----------------------------------:|:---------:|
| cohort_id   | numeric identifier of user cohort   | float     |
| cohort_name | human readable name of user cohort  | object    |
| date        | date of specific observation        | datetime  |
| endpoint    | path of URL for observation         | object    |
| end_date    | end data of user cohort             | datetime  |
| program_id  | numeric identifier for user program | float     |
| source_ip   | IP address of user machine          | object    |
| start_date  | start date of user cohort           | datetime  |
| user_id     | unique identifier for each user     | integer   |

### IV. Reproduction
---

Reproduction of these report findings requires access to the Codeup SQL database as well as curriculum access logs in the form of a `.txt` file. Please see administrator for access log files. To make use of the SQL database, you will require log in credentials stored in a file name `env.py` with variables for `host`, `username`, and `password` and the following Python function within. 

```py
def get_connection(db):
    return f'mysql+pymysql://{username}:{password}@{host}/{db}'```

[[Return to Top]](#finding-anomalies-in-curriculum-access)
