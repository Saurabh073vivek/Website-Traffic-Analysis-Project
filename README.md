# 📊 Website Traffic Analysis

A Python-based data analytics project that analyzes website traffic, user navigation behavior, page popularity, bounce rate, session duration, device usage, traffic sources, and hourly page-view patterns.

The project uses a website traffic dataset containing **400 records**, **50 unique users**, **144 unique sessions**, and **6 different website pages**.

---

## 📌 Project Overview

Understanding how users interact with a website is important for identifying popular pages, user navigation patterns, traffic sources, and areas where users may leave the website.

This project analyzes website traffic data using **Python, Pandas, Matplotlib, and Seaborn**. The dataset is processed and explored to understand:

* Popular website pages
* Entry and exit pages
* User navigation paths
* Bounce rate
* Sessions by hour
* Device usage
* Traffic by referrer source
* Pages visited per session
* Session duration
* Page views by hour

---

## 🎯 Objectives

The main objectives of this project are:

1. Analyze website traffic data.
2. Identify the most visited pages.
3. Find common entry and exit pages.
4. Understand user navigation paths within sessions.
5. Calculate the website bounce rate.
6. Analyze traffic according to the hour of the day.
7. Understand website usage across different devices.
8. Analyze traffic sources using referrer information.
9. Study the number of pages visited per session.
10. Analyze session duration.
11. Identify page-view patterns using a heatmap.

---

## 🛠️ Technologies Used

| Technology       | Purpose                                 |
| ---------------- | --------------------------------------- |
| Python           | Programming and data analysis           |
| Pandas           | Data loading, cleaning and manipulation |
| NumPy            | Numerical operations                    |
| Matplotlib       | Data visualization                      |
| Seaborn          | Statistical visualization               |
| Jupyter Notebook | Development environment                 |

---

## 📂 Project Structure

```text
Website-Traffic-Analysis/
│
├── Website Traffic Analysis Project.ipynb
├── website_traffic_data_large.csv
├── README.md
└── requirements.txt
```

---

## 📊 Dataset

The project uses a website traffic dataset containing the following columns:

| Column       | Description                                    |
| ------------ | ---------------------------------------------- |
| `user_id`    | Unique identifier of the website user          |
| `session_id` | Unique identifier of a browsing session        |
| `page`       | Website page visited by the user               |
| `timestamp`  | Date and time of the page visit                |
| `device`     | Device used to access the website              |
| `referrer`   | Source from which the user reached the website |

### Dataset Summary

| Metric          |               Value |
| --------------- | ------------------: |
| Total Records   |                 400 |
| Unique Users    |                  50 |
| Unique Sessions |                 144 |
| Unique Pages    |                   6 |
| Start Date      | 2025-06-01 08:24:00 |
| End Date        | 2025-06-08 05:42:00 |

### Website Pages

The dataset contains these six pages:

```text
/home
/products
/about
/blog
/contact
/checkout
```

---

# 🔄 Project Workflow

The project follows this data analysis workflow:

```text
Website Traffic Dataset
        ↓
Data Loading
        ↓
Data Preprocessing
        ↓
Basic Data Summary
        ↓
Page Analysis
        ↓
User Navigation Analysis
        ↓
Bounce Rate Calculation
        ↓
Time Analysis
        ↓
Device Analysis
        ↓
Referrer Analysis
        ↓
Session Analysis
        ↓
Heatmap Visualization
        ↓
Website Traffic Insights
```

---

# 🧹 Data Collection & Preprocessing

The dataset is loaded using Pandas:

```python
df = pd.read_csv("website_traffic_data_large.csv")
```

The timestamp column is converted into datetime format:

```python
df = pd.read_csv(
    "website_traffic_data_large.csv",
    parse_dates=["timestamp"]
)
```

The data is then sorted by session and timestamp:

```python
df.sort_values(
    by=["session_id", "timestamp"],
    inplace=True
)
```

This ordering is important for analyzing the sequence of pages visited by users.

---

# 📋 Basic Data Summary

The notebook calculates:

* Total records
* Unique users
* Unique sessions
* Number of pages
* Dataset time range

The dataset contains:

```text
400 Records
50 Unique Users
144 Unique Sessions
6 Pages
```

---

# 📈 Analysis Performed

## 1. Popular Pages

The project calculates the number of times each page was visited using:

```python
popular_pages = df["page"].value_counts()
```

The top pages are visualized using a bar chart.

The page-view counts in the dataset are:

| Page        | Page Views |
| ----------- | ---------: |
| `/blog`     |         77 |
| `/checkout` |         75 |
| `/home`     |         65 |
| `/contact`  |         62 |
| `/products` |         61 |
| `/about`    |         60 |

---

## 2. Entry & Exit Pages

The first page visited in each session is identified as the **entry page**, while the final page is identified as the **exit page**.

The notebook uses:

```python
entry_pages = df.groupby("session_id").first()["page"].value_counts()

exit_pages = df.groupby("session_id").last()["page"].value_counts()
```

Separate visualizations are created for:

* Top Entry Pages
* Top Exit Pages

---

## 3. User Navigation Patterns

The project creates a navigation path for every session.

For example:

```text
/home → /products → /checkout
```

The navigation path is generated using:

```python
paths = df.groupby("session_id")["page"].apply(
    lambda x: " → ".join(x)
).reset_index()
```

This allows user movement through the website to be examined at the session level.

---

# 📉 4. Bounce Rate

A session containing only one page view is considered a bounce in this project.

The calculation is:

```python
session_page_counts = df.groupby("session_id")["page"].count()

bounces = session_page_counts[
    session_page_counts == 1
].count()

bounce_rate = (
    bounces / df["session_id"].nunique()
) * 100
```

### Calculated Bounce Rate

```text
21.53%
```

This means that approximately **21.53% of the 144 sessions** contained only one page view according to the project's definition.

---

# ⏰ 5. Time Analysis

The hour is extracted from the timestamp:

```python
df["hour"] = df["timestamp"].dt.hour
```

The project then calculates unique sessions for each hour:

```python
hourly_sessions = df.groupby(
    "hour"
)["session_id"].nunique()
```

A line chart titled:

**Sessions by Hour of Day**

is used to visualize hourly traffic patterns.

---

# 📱 6. Device Usage

The project analyzes the devices used to access the website.

```python
device_counts = df["device"].value_counts()
```

The dataset contains:

| Device  | Records |
| ------- | ------: |
| Mobile  |     134 |
| Tablet  |     134 |
| Desktop |     132 |

A bar chart is used to visualize device usage distribution.

---

# 🔗 7. Referrer Source Analysis

The project analyzes where website traffic comes from using the `referrer` column.

```python
referrer_counts = df["referrer"].value_counts()
```

The dataset contains the following referrer sources:

| Referrer      | Records |
| ------------- | ------: |
| linkedin.com  |      76 |
| direct        |      71 |
| twitter.com   |      71 |
| instagram.com |      68 |
| google.com    |      61 |
| facebook.com  |      53 |

A bar chart is created to visualize traffic by referrer source.

---

# 📄 8. Pages per Session

The project calculates the number of pages visited in each session:

```python
session_pages = df.groupby(
    "session_id"
)["page"].count()
```

A histogram with KDE is used to visualize the distribution of pages visited per session.

The dataset has an average of approximately:

```text
2.78 pages per session
```

---

# ⏱️ 9. Session Duration

Session duration is calculated using the first and last timestamps of each session.

```python
session_times = df.groupby(
    "session_id"
)["timestamp"].agg(["min", "max"])

session_times["duration_minutes"] = (
    session_times["max"] -
    session_times["min"]
).dt.total_seconds() / 60
```

A histogram is then used to visualize the distribution of session durations.

---

# 🔥 10. Page Views vs Hour of Day Heatmap

The project creates a heatmap showing page views according to:

* Hour of the day
* Website page

```python
heatmap_data = df.groupby(
    ["hour", "page"]
).size().unstack(fill_value=0)
```

This visualization helps examine when different website pages receive page views.

---

# 📊 Visualizations Included

The Jupyter Notebook contains visualizations for:

1. **Top 10 Visited Pages**
2. **Top Entry Pages**
3. **Top Exit Pages**
4. **Sessions by Hour of Day**
5. **Device Usage Distribution**
6. **Traffic by Referrer Source**
7. **Distribution of Pages Visited per Session**
8. **Session Duration Distribution**
9. **Heatmap of Page Views by Hour**

---

# 💡 Key Findings from the Dataset

Based on the uploaded dataset and notebook calculations:

* The dataset contains **400 website activity records**.
* There are **50 unique users**.
* There are **144 unique sessions**.
* The website contains **6 tracked pages**.
* `/blog` has the highest number of recorded page views with **77 views**.
* `/about` has **60 recorded page views**.
* The calculated bounce rate is **21.53%**.
* Average pages visited per session are approximately **2.78**.
* Mobile and tablet each account for **134 records**, while desktop accounts for **132 records**.
* `linkedin.com` is the most frequent referrer in the dataset with **76 records**.

---

# 🚀 How to Run the Project

## Step 1: Clone the Repository

```bash
git clone https://github.com/your-username/website-traffic-analysis.git
```

## Step 2: Open the Project

```bash
cd website-traffic-analysis
```

## Step 3: Install Dependencies

```bash
pip install -r requirements.txt
```

## Step 4: Start Jupyter Notebook

```bash
jupyter notebook
```

## Step 5: Open the Notebook

Open:

```text
Website Traffic Analysis Project.ipynb
```

## Step 6: Keep the Dataset in the Same Directory

```text
website_traffic_data_large.csv
```

## Step 7: Run the Notebook

Execute the cells sequentially from the beginning to reproduce the analysis and visualizations.

---

# 📦 Requirements

Create a `requirements.txt` file:

```text
numpy
pandas
matplotlib
seaborn
jupyter
```

Install all dependencies:

```bash
pip install -r requirements.txt
```

---

# 🔮 Future Scope

The current notebook focuses on exploratory website traffic analysis. It can be extended by adding:

* Interactive dashboards using Power BI
* Real-time website analytics
* Conversion-rate analysis
* User segmentation
* Advanced session-path analysis
* Traffic forecasting
* Machine learning-based user behavior prediction
* Automated reporting
* Google Analytics API integration
* Interactive web-based analytics dashboard

---

# 🎓 Skills Demonstrated

This project demonstrates practical experience with:

* Python
* Pandas
* NumPy
* Matplotlib
* Seaborn
* Data preprocessing
* Exploratory Data Analysis (EDA)
* Session-based analysis
* Time-series analysis
* User behavior analysis
* Data visualization
* Statistical interpretation

---

# 👨‍💻 Author

**Saurabh Vivek**

B.Tech – Computer Science and Engineering
Jodhpur Institute Of Engineering & Technology (JIET), Jodhpur, Rajasthan

---

# 📌 Project Information

**Project Name:** Website Traffic Analysis

**Project Type:** Data Analytics / Exploratory Data Analysis

**Dataset Size:** 400 Records

**Analysis Environment:** Jupyter Notebook

**Programming Language:** Python

**Libraries:** Pandas, NumPy, Matplotlib, Seaborn

---

## ⭐ Conclusion

This project demonstrates a complete exploratory data analysis workflow for website traffic data.

By analyzing page views, navigation paths, bounce rate, session duration, hourly traffic, device usage, referrer sources, and page-view patterns, the project provides a structured view of how users interact with the website.

The project can serve as a foundation for developing a more advanced website analytics and business intelligence solution.

