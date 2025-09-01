covid_tracker/
│── data/                     # store dataset (if downloaded)
│   └── covid_data.csv
│── covid_tracker.py          # main analysis script
│── visualizations.py         # plotting functions
│── requirements.txt          # dependencies
│── README.md                 # project description
│── web_app/                  # (optional Django/Flask app)
import pandas as pd
import requests
import matplotlib.pyplot as plt
import seaborn as sns

# =============================
# 1. Load COVID-19 data from API
# =============================
url = "https://disease.sh/v3/covid-19/countries"
try:
    response = requests.get(url)
    data = response.json()
    df = pd.DataFrame(data)
    print("✅ Data fetched successfully!")
except Exception as e:
    print("❌ Error fetching data:", e)
    exit()

# =============================
# 2. Basic Exploration
# =============================
print("\n📌 First 5 rows:")
print(df[["country", "cases", "deaths", "recovered", "todayCases", "todayDeaths"]].head())

print("\n📊 Summary Stats:")
print(df[["cases", "deaths", "recovered"]].describe())

# =============================
# 3. Find Insights
# =============================
top_cases = df.sort_values(by="cases", ascending=False).head(5)
top_deaths = df.sort_values(by="deaths", ascending=False).head(5)

print("\n🏆 Top 5 Countries by Cases:")
print(top_cases[["country", "cases"]])

print("\n⚠️ Top 5 Countries by Deaths:")
print(top_deaths[["country", "deaths"]])

# =============================
# 4. Visualization
# =============================
sns.set(style="whitegrid")

# Bar chart - top 10 countries by cases
plt.figure(figsize=(10,6))
sns.barplot(x="cases", y="country", data=df.sort_values("cases", ascending=False).head(10), palette="Reds")
plt.title("Top 10 Countries by COVID-19 Cases")
plt.xlabel("Total Cases")
plt.ylabel("Country")
plt.show()

# Pie chart - distribution of cases among top 5
plt.figure(figsize=(6,6))
plt.pie(top_cases["cases"], labels=top_cases["country"], autopct="%1.1f%%", startangle=140, colors=sns.color_palette("Set2"))
plt.title("Cases Distribution (Top 5 Countries)")
plt.show()
# project-plp-py
