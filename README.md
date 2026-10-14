# -google-play-store-

import matplotlib.pyplot as plt
import pandas as pd
from pathlib import Path

base = Path(__file__).parent
apps = pd.read_csv(base / 'googleplaystore.csv')
reviews = pd.read_csv(base / 'googleplaystore_user_reviews.csv')

print(apps.shape)
print(reviews.shape)
print(apps.head())
print(reviews.head())
print(apps.head(3))
print(reviews.head(3))
print(apps.tail(3))
print(reviews.tail(3))

print(apps.describe(include='all'))
print(reviews.describe(include='all'))
print(apps.columns)
print(reviews.columns)
print(apps['Rating'].mean())
print(apps['Category'].unique())
avg = apps.groupby('Category')['Rating'].mean().sort_values(ascending=False)
print(avg.head(10))
five_star = apps[apps['Rating'] == 5.0]
print(five_star[['App', 'Category', 'Reviews']])
print(len(five_star))
print(apps['Type'].value_counts())

print(reviews.columns)
print(apps.columns)

apps['Reviews'] = pd.to_numeric(apps['Reviews'], errors='coerce')

max_reviews_app = apps.loc[apps['Reviews'].idxmax()]

print(max_reviews_app['App'])
print(max_reviews_app['Reviews'])


apps = apps[apps['Category'] != '1.9']
apps = apps.drop_duplicates(subset='App')

apps['Reviews'] = pd.to_numeric(apps['Reviews'], errors='coerce')

apps = (apps.sort_values('Reviews', ascending=False)
            .drop_duplicates(subset='App', keep='first')
            .reset_index(drop=True))

print(apps.shape)
print(reviews.shape)
print(apps.head())
print(reviews.head())
print(apps.head(3))
print(reviews.head(3))
print(apps.tail(3))
print(reviews.tail(3))

print(apps.describe(include='all'))
print(reviews.describe(include='all'))
print(apps.columns)
print(reviews.columns)
print(apps['Rating'].mean())
print(apps['Category'].unique())
avg = apps.groupby('Category')['Rating'].mean().sort_values(ascending=False)
print(avg.head(10))
five_star = apps[apps['Rating'] == 5.0]
print(five_star[['App', 'Category', 'Reviews']])
print(len(five_star))
print(apps['Type'].value_counts())

print(reviews.columns)
print(apps.columns)

apps['Reviews'] = pd.to_numeric(apps['Reviews'], errors='coerce')

max_reviews_app = apps.loc[apps['Reviews'].idxmax()]

print(max_reviews_app['App'])
print(max_reviews_app['Reviews'])

print(apps[apps['Rating'] == 5.0]['App'].nunique())

ratings = apps['Rating'].dropna()

plt.figure(figsize=(8, 5))
plt.hist(ratings, bins=20, color="#4F4C73", edgecolor='white')
plt.axvline(ratings.mean(), color='red', linestyle='--',
            label=f'Mean = {ratings.mean():.2f}')
plt.title('Distribution of App Ratings')
plt.xlabel('Rating')
plt.ylabel('Number of apps')
plt.legend()
plt.show()

top10 = apps['Category'].value_counts().head(10).sort_values()

plt.figure(figsize=(8, 5))
plt.barh(top10.index, top10.values, color="#DB3D7C")
plt.title('Top 10 Categories by Number of Apps')
plt.xlabel('Number of apps')
plt.tight_layout()
plt.show()

apps['Reviews'] = pd.to_numeric(apps['Reviews'], errors='coerce')

print(apps['Reviews'].mean())

print(apps['Type'].value_counts())
print(apps['Type'].value_counts(normalize=True) * 100)

print(apps.nlargest(10, 'Reviews')[['App', 'Category', 'Rating', 'Reviews']])
top10 = apps.nlargest(10, 'Reviews').sort_values('Reviews')

plt.figure(figsize=(9, 5))
plt.barh(top10['App'].str[:25], top10['Reviews'] / 1e6, color="#94C99E")
plt.xlabel('Reviews (millions)')
plt.title('Top 10 Apps by Number of Reviews')
plt.tight_layout()
plt.show()

print(apps.groupby('Type')['Rating'].mean())
avg = apps.groupby('Type')['Rating'].mean()

plt.figure(figsize=(5, 4))
plt.bar(avg.index, avg.values, color=['#3B7DDD', "#AF52D1"])
plt.ylim(3.8, 4.4)
plt.ylabel('Average rating')
plt.title('Average Rating: Free vs Paid')
plt.show()
