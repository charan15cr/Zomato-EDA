Zomato EDA
---

**Data Source**: Keggle

**Environment Used for EDA**: Python (google colab) https://colab.research.google.com/drive/1nZzTwi1pyj9seJcVShHtvCWcsbPwuHa3?usp=sharing , Excel

**The notebook has two datasets**:

zomato.csv – Contains restaurant-related data such as names, ratings, locations, and cuisines etc.

Country Code.xlsx – Contains maps country codes to country names for international data analysis.

---

**Problem Statement**: Analysis of Zomato Restaurant Data Zomato, one of the leading online food delivery and restaurant discovery platforms, hosts a vast amount of data on restaurants, customer ratings, cuisines, pricing, and locations. Analyzing this data can provide valuable insights into customer preferences, restaurant performance.

**Objective**:


The goal of this analysis is to explore the Zomato dataset to uncover key trends and patterns related to restaurant ratings, pricing strategies, and customer preferences. Specifically, we aim to answer the following questions:

What factors influence restaurant ratings the most?

How does restaurant pricing vary across different locations and cuisines?

What are the most popular cuisines and locations on Zomato?

Is there any correlation between restaurant ratings and pricing?

Are there any patterns in customer preferences that can help restaurants improve their services?

This exploratory analysis will provide actionable insights for restaurant owners, food industry analysts, and Zomato itself to optimize their offerings and improve customer satisfaction.

**operations performed in the analysis**:

1. Data Cleaning  
2. Handling Missing Values  
3. Data Visualization  
4. Value Counts and Grouping  
5. Merging Datasets  
6. Label Encoding  
7. Filtering and Sorting Data  
8. Plotting Pie Charts and Bar Graphs

---

The dataset has **9,551 records and 21 columns**, covering restaurant details like name, location, cuisines, pricing, and ratings. A separate **Country Code** dataset maps country codes to country names. Most data is complete, with minor missing values in the "Cuisines" column.

![Image](https://github.com/user-attachments/assets/7178a38b-55bd-4fee-b1b9-1273a9511342)

## Sample Screenshots and Code Snippets

1. The dataset shows an **average cost for two at 1,199**, but the high standard deviation suggests significant price variation. **Ratings range from 0 to 4.9**, with a median of **3.2**, indicating most restaurants have mid-range ratings. **Votes vary widely**, from **0 to 10,934**, showing differences in customer engagement levels.

![Image](https://github.com/user-attachments/assets/a80ba2ab-063e-4d0d-80bc-f730c8e4f534)

2. The **correlation** heatmap shows that **aggregate rating and price range (0.44)** have a moderate positive relationship, suggesting higher-rated restaurants tend to be pricier. **Votes and ratings (0.31)** are also positively correlated, indicating more popular restaurants often receive better ratings. Other correlations are weak, implying minimal direct relationships between features like cost, country, and ratings.

```http
# Select only numerical features for correlation analysis
numerical_features = df_zomato.select_dtypes(include=np.number).columns
df_zomato_numerical = df_zomato[numerical_features]

# Calculate the correlation matrix

correlation_matrix = df_zomato_numerical.corr()

plt.figure(figsize=(4,4))
sns.heatmap(correlation_matrix,
            annot=True,
            linewidth=1)
plt.show()
```

![Image](https://github.com/user-attachments/assets/b0bcc151-13ad-4247-b53a-af3ec528c764)

3. The **pie chart** shows that 39.13% of restaurants have an "Average" rating, making it the most common category. 22.49% of restaurants are "Not Rated," indicating a significant portion lacks user feedback. Only 3.15% of restaurants are rated "Excellent," suggesting high ratings are rare.

```http
plt.figure(figsize = (3,3))
plt.title("Percentage of Resturant Ratings", fontsize = 10, fontweight = 'bold')
gb = df_zomato.groupby("Ratings").agg({'Ratings' : "count"})
plt.pie(gb['Ratings'], labels = gb.index, autopct = "%1.2f%%")
plt.show()
```

![Image](https://github.com/user-attachments/assets/37f745ed-85c1-41c7-8d8d-0b0b49e65ba7)

4. **Restaurant Rating Color Categories**  
The table categorizes restaurant ratings by color, where **Dark Green (4.5-4.9) represents "Excellent" ratings**, while **Green (4.0-4.4) signifies "Very Good."** Orange (2.5-3.4) corresponds to "Average," Red (1.8-2.4) to "Poor," and White (0.0) represents restaurants that are "Not Rated." The Yellow category (3.5-3.9) represents "Good" ratings.
```http
result_rating = subset_df.groupby('Rating color')[['Aggregate rating', 'Ratings']].aggregate(['min', 'max'])
result_rating
```

**Aggregate Rating Distribution**  
The box plot shows that **75% of restaurant ratings fall between 2.5 and 3.5**, meaning most restaurants receive "Average" ratings. There are some outliers below 2.5, indicating a few poorly rated restaurants, while the upper range extends to just under 5, representing the highest-rated restaurants.
```http
plt.figure(figsize = (5,2))
sns.boxplot(data = df_zomato, x = 'Aggregate rating')
# 75 percent of the agg rating lie between 2.5 to 3.5
```

![Image](https://github.com/user-attachments/assets/af1b0c21-192f-444c-a130-efdfd73d093b)

5. **Joining Restaurant and Country Data**  
The dataset merges restaurant data (`df_zomato`) with country details (`df_Countrycode`) using an inner join on the "Country Code" column. This operation ensures that only records with matching country codes in both datasets are included, allowing for a comprehensive view of restaurant locations along with their corresponding country details.  
```http
#join operation
join_df = pd.merge(left = df_zomato,
                   right = df_Countrycode,
                   left_on = 'Country Code',
                   right_on = 'Country Code',
                   how = 'inner')
join_df.head()
```

![Image](https://github.com/user-attachments/assets/b5f5111d-5cc9-473e-b29b-ce7d22d92659)

6. **Online Delivery Availability**  
The bar chart illustrates the distribution of restaurants offering online delivery. Out of the total, **7,100 restaurants do not provide online delivery**, whereas **2,451 restaurants do**. This indicates that a significant majority of restaurants still rely on dine-in or takeaway services rather than online ordering.
```http
plt.figure(figsize = (4,4))
plt.title("Resturants Available For Online Delivery", fontsize = 10)
ax= sns.countplot(x = 'Online_Delivery', data = join_df)
ax.bar_label(ax.containers[0])
plt.show()
```
![Image](https://github.com/user-attachments/assets/8f710cc2-0a74-4f9b-8bf2-efda950386ba)

7. **Zomato's Presence in Top 3 Countries**  
The pie chart highlights Zomato's restaurant distribution across three countries. **India dominates with 94.39%**, while the **United States accounts for 4.73%** and the **United Kingdom only 0.87%**. This shows Zomato's strong presence in India compared to other major markets.
```http
plt.figure(figsize = (4,4))
plt.title("Zomato in Top 3 Country", fontsize = 10, fontweight = 'bold')
plt.pie(country_values[:3], labels = country_names[:3], autopct = "%1.3f%%")
plt.show()
```
![Image](https://github.com/user-attachments/assets/a4cb101c-45e1-48eb-b124-52dddd0be008)

8. **Online Delivery Availability by Country**  
The bar chart reveals **India** as the dominant market for Zomato, with the highest number of restaurants offering online delivery. Other countries, including **the United States, UAE, and the United Kingdom**, have significantly fewer restaurants, with a small portion supporting online delivery. This suggests Zomato’s primary focus on the Indian market for online food delivery services.
```http
data = {
    "Country": ["Australia", "Brazil", "Canada", "India", "India", "Indonesia", "New Zealand", "Phillipines", "Qatar",
                "Singapore", "South Africa", "Sri Lanka", "Turkey", "UAE", "UAE", "United Kingdom", "United States"],
    "Online_Delivery": ["No", "No", "No", "No", "Yes", "No", "No", "No", "No", "No", "No", "No", "No", "No", "Yes", "No", "No"],
    "Count": [24, 60, 4, 6229, 2423, 21, 40, 22, 20, 20, 60, 20, 34, 32, 28, 80, 434]
}

df = pd.DataFrame(data)

df_pivot = df.pivot(index="Country", columns="Online_Delivery", values="Count").fillna(0)

df_pivot.plot(kind="bar", stacked=True, figsize=(8, 6), colormap="viridis")

plt.title("Online Delivery Availability by Country")
plt.xlabel("Country")
plt.ylabel("Number of Restaurants")
plt.xticks(rotation=45)
plt.legend(title="Online Delivery")

plt.tight_layout()
plt.show()
```
![Image](https://github.com/user-attachments/assets/a15078f6-cda4-4705-bc4d-bdc345f6e549)

9. **Top 3 Indian Cities with Maximum Zomato Listings**

New Delhi dominates Zomato listings among the top 3 cities with 71.35%, followed by Gurgaon at 14.57% and Noida at 14.08%. This shows a heavy concentration of restaurants and activity in the capital city.
```http
plt.figure(figsize=(3, 3))
plt.title("Top 3 Cities in India")
plt.pie(city_count.values[:3],
        labels=city_count.index[:3],
        autopct="%1.2f%%")
plt.show()
```
![Image](https://github.com/user-attachments/assets/ead50cbe-a3b4-49ae-b010-ed7a098d0ebc)

10. **Table Availability in Restaurants**  
Only 1,158 restaurants offer table reservations, while 8,393 do not. This suggests that most listings on Zomato cater to casual or quick dining experiences.

**Distribution of Ratings**  
The majority of restaurants fall under the "Average" category with 3,737 counts, followed by "Not Rated" (2,148) and "Good" (2,100). Very few restaurants are rated "Excellent" or "Poor".

**Rating Colors Overview**  
The color distribution mirrors the rating pattern, with most restaurants marked in "Orange" (Average), "White" (Not Rated), and "Yellow" (Good). Very few have "Dark Green" or "Red" tags, indicating rare extremes.
```http
columns = ['Tables_Available', 'Ratings', 'Rating color']

fig, axes = plt.subplots(1, 3, figsize=(10, 5))

rating_palette = ['green', 'lightgreen', 'yellow', 'orange', 'gray', 'red']

# Loop through columns and create count plots
for i, col in enumerate(columns):
    if col == "Rating color":
        ax = sns.countplot(x=join_df[col], data=join_df, ax=axes[i], palette=rating_palette)
    else:
        ax = sns.countplot(x=join_df[col], data=join_df, ax=axes[i])

    axes[i].set_title(f"Count of {col}")
    axes[i].set_xlabel("")
    axes[i].set_ylabel("Count")
    axes[i].tick_params(axis='x', rotation=45)

    for p in ax.patches:
        ax.annotate(f'{int(p.get_height())}',
                    (p.get_x() + p.get_width() / 2, p.get_height()),
                    ha='center', va='bottom', fontsize=10)

plt.tight_layout()
plt.show()
```
![Image](https://github.com/user-attachments/assets/437b5093-a60e-476b-bfc2-9a06fb62d2e7)

11. **Zomato Price Ranges in India**  
The majority of restaurants on Zomato fall under the most affordable price range (1), followed by price range 2. Higher-end options (price ranges 3 and 4) are significantly fewer, indicating a strong preference or availability of budget-friendly dining options in India.
```https
plt.figure(figsize=(4, 4))
plt.title("Zomato Price Ranges in India")
sns.countplot(x="Price range", data=join_df, palette='rocket_r')
plt.show()
```
![Image](https://github.com/user-attachments/assets/18dd3df3-84ff-40f5-a502-caa0a9506c04)

---

**Overall Insights from Zomato EDA**

The **EDA** reveal that Zomato is primarily used in metro cities, with New Delhi alone contributing over 70% of listings. Most restaurants do not offer table booking, indicating a focus on quick-service or casual dining. The majority are rated "Average" or remain unrated, with very few marked "Excellent" or "Poor." Price-wise, affordable options dominate the platform, catering to cost-conscious consumers. Altogether, Zomato seems to serve a value-driven urban audience looking for accessible experiences.

---
**Linkedin Profile** : www.linkedin.com/in/charan-r-b04875358
