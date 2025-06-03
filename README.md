LEVEL 1
# Task-Identifying the top 3 most common cuisines and calculating their percentage among all restaurants.

---

### ✅ **Code:**

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from collections import Counter

# Load dataset
df = pd.read_csv("Dataset  (2).csv")

# Drop missing cuisine entries
cuisine_data = df['Cuisines'].dropna()

# Split and normalize cuisine names
all_cuisines = cuisine_data.str.split(', ').sum()
cuisine_counts = Counter(all_cuisines)

# Get the top 3 cuisines
top_cuisines = cuisine_counts.most_common(3)

# Calculate total number of restaurants
total_restaurants = len(df)

# Calculate percentage for each top cuisine
top_cuisine_percentages = [(cuisine, count, round((count / total_restaurants) * 100, 2)) 
                           for cuisine, count in top_cuisines]

# Display the result
for cuisine, count, perc in top_cuisine_percentages:
    print(f"{cuisine}: {count} restaurants ({perc}%)")

# Prepare data for visualization
cuisine_names = [item[0] for item in top_cuisine_percentages]
cuisine_percentages = [item[2] for item in top_cuisine_percentages]

# Plotting
plt.figure(figsize=(8, 6))
sns.barplot(x=cuisine_names, y=cuisine_percentages, palette="Set2")

# Labels and title
plt.title("Top 3 Most Common Cuisines", fontsize=14)
plt.xlabel("Cuisine", fontsize=12)
plt.ylabel("Percentage of Restaurants (%)", fontsize=12)
plt.ylim(0, max(cuisine_percentages) + 10)

# Add value labels
for i, perc in enumerate(cuisine_percentages):
    plt.text(i, perc + 1, f"{perc}%", ha='center', va='bottom', fontsize=10)

plt.tight_layout()
plt.show()
```

---

### 📊 **Output (Printed Result):**

```
North Indian: 3960 restaurants (41.46%)
Chinese: 2735 restaurants (28.64%)
Fast Food: 1986 restaurants (20.79%)
```

### 📉 **Output (Bar Chart):**

* A bar chart showing the percentages of the top 3 cuisines:

  * **North Indian**
  * **Chinese**
  * **Fast Food**
![task 1 top cusine](https://github.com/user-attachments/assets/5c999940-69d2-4c75-bd56-0c41f9664e6c)
![plot (task 1)](https://github.com/user-attachments/assets/34d56f12-367c-4617-9d04-49826b852980)


TASK 2
**City Analysis** 

1. ✅ Identifying the city with the highest number of restaurants.
2. ✅ Calculating the average rating of restaurants for each city.
3. ✅ Finding the city with the highest average rating.
4. 📊 Visualizations for both results.

---

### ✅ **Python Code (for Spyder):**

```python
# Task: City Analysis - Restaurant Count & Average Ratings

import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load dataset
df = pd.read_csv("Dataset  (2).csv")

# -----------------------------
# 1. City with the most restaurants
# -----------------------------
city_counts = df['City'].value_counts()
top_city = city_counts.idxmax()
top_count = city_counts.max()

print(f"City with the highest number of restaurants: {top_city} ({top_count} restaurants)")

# -----------------------------
# 2. Average rating per city
# -----------------------------
avg_ratings = df.groupby('City')['Aggregate rating'].mean().sort_values(ascending=False)
top_avg_rating_city = avg_ratings.idxmax()
top_avg_rating = round(avg_ratings.max(), 2)

print(f"City with the highest average rating: {top_avg_rating_city} ({top_avg_rating})")

# -----------------------------
# 3. Plot: Top 10 Cities by Number of Restaurants
# -----------------------------
plt.figure(figsize=(10, 6))
sns.barplot(x=city_counts.head(10).values, y=city_counts.head(10).index, palette="Blues_d")
plt.title("Top 10 Cities by Number of Restaurants")
plt.xlabel("Number of Restaurants")
plt.ylabel("City")
plt.tight_layout()
plt.show()

# -----------------------------
# 4. Plot: Top 10 Cities by Average Rating
# -----------------------------
plt.figure(figsize=(10, 6))
sns.barplot(x=avg_ratings.head(10).values, y=avg_ratings.head(10).index, palette="Greens_d")
plt.title("Top 10 Cities by Average Restaurant Rating")
plt.xlabel("Average Rating")
plt.ylabel("City")
plt.tight_layout()
plt.show()
```

---

### 📌 How to Use in Spyder:

1. Open **Spyder** from Anaconda Navigator or your desktop.
2. Ensure the file `Dataset  (2).csv` is in your **current working directory**.
3. Copy and paste the above code into the editor.
4. Click **Run** (green play button ▶️).
5. You will see:

   * Two printed statements in the **console**.
   * Two **bar charts** in the **Plots** pane.

---
![task 2 (city analysis)](https://github.com/user-attachments/assets/a36349c9-adf4-4236-97f5-67bfd71c7c6b)
![task 2(plots)](https://github.com/user-attachments/assets/fde35b75-a144-4332-b592-4b513efc0ca8)

TASK 3
**Task: Price Range Distribution**
Its including

1. ✅ Histogram or Bar Chart of price ranges
2. ✅ Percentage calculation for each price range category
3. ✅ CSV export option (optional)

---

### ✅ **Code for Price Range Distribution (Spyder)**

```python
# Task: Price Range Distribution

import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load the dataset
df = pd.read_csv("Dataset  (2).csv")

# -----------------------------
# 1. Count and percentage of price ranges
# -----------------------------
price_counts = df['Price range'].value_counts().sort_index()
total_restaurants = len(df)
price_percentages = (price_counts / total_restaurants * 100).round(2)

# Display results
print("Price Range Distribution (Counts and Percentages):")
for price, count in price_counts.items():
    print(f"Price Range {price}: {count} restaurants ({price_percentages[price]}%)")

# -----------------------------
# 2. Plot: Bar Chart of Price Range Distribution
# -----------------------------
plt.figure(figsize=(8, 6))
sns.barplot(x=price_counts.index, y=price_percentages.values, palette="pastel")

plt.title("Price Range Distribution Among Restaurants")
plt.xlabel("Price Range (1 = Low, 4 = High)")
plt.ylabel("Percentage of Restaurants (%)")
plt.ylim(0, max(price_percentages.values) + 5)

# Annotate percentages on bars
for i, perc in enumerate(price_percentages.values):
    plt.text(i, perc + 0.5, f"{perc}%", ha='center', fontsize=10)

plt.tight_layout()
plt.show()

# -----------------------------
# 3. Optional: Export to CSV
# -----------------------------
price_df = pd.DataFrame({
    "Price Range": price_counts.index,
    "Number of Restaurants": price_counts.values,
    "Percentage": price_percentages.values
})
price_df.to_csv("task3_price_range_distribution.csv", index=False)
print("✅ Task 3 data exported to task3_price_range_distribution.csv")
```

---

### 📊 Output:

* Bar chart showing the percentage of restaurants in each **price range (1 to 4)**.
* Printed text summary (example):

  ```
  Price Range 1: 4410 restaurants (46.14%)
  Price Range 2: 3682 restaurants (38.53%)
  ...
  ```

---
![task3 price range (using bar chart)](https://github.com/user-attachments/assets/bd76f86a-c2e6-4973-a0c8-b0a108b89292)



TASK 4
### ✅ Code to Analyze & Visualize Online Delivery Impact

It includes,

1. Calculates the percentage of restaurants offering online delivery.
2. Compares the average ratings between restaurants with and without online delivery.
3. Visualizes both results with **bar plots**.

---



```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load the dataset
file_path = "Dataset  (2).csv"  # Make sure the file is in the same directory
df = pd.read_csv(file_path)

# Set seaborn style
sns.set(style="whitegrid")

# ----------------------------------------
# 1. Percentage of Online Delivery
# ----------------------------------------
online_delivery_counts = df['Has Online delivery'].value_counts()
total = len(df)
online_delivery_percentage = (online_delivery_counts.get('Yes', 0) / total) * 100
print(f"✅ Percentage of restaurants with online delivery: {online_delivery_percentage:.2f}%")

# Visualization - Pie Chart
plt.figure(figsize=(6, 6))
plt.pie(online_delivery_counts, labels=online_delivery_counts.index,
        autopct='%1.1f%%', startangle=140, colors=sns.color_palette('pastel'))
plt.title("Online Delivery Availability")
plt.axis('equal')
plt.tight_layout()
plt.show()

# ----------------------------------------
# 2. Average Ratings Comparison
# ----------------------------------------
avg_ratings = df.groupby('Has Online delivery')['Aggregate rating'].mean()
print("\n✅ Average Ratings:")
print(avg_ratings)

# Visualization - Bar Plot
plt.figure(figsize=(6, 5))
sns.barplot(x=avg_ratings.index, y=avg_ratings.values, palette="Set2")
plt.title("Average Ratings: With vs Without Online Delivery")
plt.ylabel("Average Rating")
plt.xlabel("Online Delivery")
plt.ylim(0, 5)
plt.tight_layout()
plt.show()
```

---

### 🖼️ What You’ll See:

1. **Pie Chart** showing how many restaurants offer online delivery (`Yes` vs `No`).
2. **Bar Plot** comparing the **average rating** of restaurants with and without online delivery.
3. **Console Output**:

   ```
   ✅ Percentage of restaurants with online delivery: 25.66%
   ✅ Average Ratings:
   No     2.47
   Yes    3.25
   ```
![task4(online delivery)](https://github.com/user-attachments/assets/dcf5bd03-0043-4ab1-8fa2-7ecb87271c51)
![task4 (online)](https://github.com/user-attachments/assets/fbac0d62-4051-411e-bbee-53b72cf038d0)


