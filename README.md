# StrataScratch Analytical Questions | Python-Pandas

#### View StrataScratch Analytical Questions | Python-Pandas, here: [stratascratch.com](https://platform.stratascratch.com/coding?code_type=2&is_freemium=1&order_field=difficulty)

#### [Easy](https://github.com/s1dewalker/StrataScratch-Python-Pandas) | [Medium](https://github.com/s1dewalker/StrataScratch-Python-Pandas-2) | Hard
<br/>

## [Top 5 States With 5 Star Businesses](https://platform.stratascratch.com/coding/10046-top-5-states-with-5-star-businesses?code_type=2)

```python
reqd_df = yelp_business[yelp_business['stars'] == 5]

g_df = reqd_df.groupby('state', as_index = False).agg(stars5 = ('stars','size'))

sorted_df = g_df.sort_values(['stars5','state'], ascending = [False,True])

sorted_df['rank'] = sorted_df['stars5'].rank(method = 'min', ascending = False)

sorted_df[sorted_df['rank'] <=5][['state','stars5']]
```
<br/>

## [Host Popularity Rental Prices](https://platform.stratascratch.com/coding/9632-host-popularity-rental-prices?code_type=2)

```python
def pop(x):
    if x == 0:
        return "New"
    elif 1 <= x <= 5:
        return "Rising"
    elif 6 <= x <= 15:
        return "Trending Up"
    elif 16 <= x <= 40:
        return "Popular"
    else:
        return "Hot"
        
df2['popularity'] = df2['number_of_reviews'].apply(pop)


df2.groupby('popularity', as_index = False).agg(
    min_price = ('price', 'min'),
    avg_price = ('price', 'mean'),
    max_price = ('price', 'max')
    )
```
Notes: Check logical operators in function (& has higher precedence than >= and <=, so 1 & x gets evaluated first, leading to unexpected behavior.)
<br/>


## [City With Most Amenities](https://platform.stratascratch.com/coding/9633-city-with-most-amenities?code_type=2)

```python
df["amenities_count"] = df["amenities"].str.count(",") + 1

grouped_df = df.groupby('city', as_index = False).agg(total_am = ('amenities_count', 'sum'))

grouped_df[grouped_df['total_am'] == grouped_df['total_am'].max()][['city']]
```
Notes: We can count the number of amenities in each row by using the `str.count()` function on the 'amenities' column. We can then create a new column called 'amenities_count' to store the count for each row.
<br/>

## [Top Percentile Fraud](https://platform.stratascratch.com/coding/10303-top-percentile-fraud?code_type=2)

```python
fraud_score["percentile"] = fraud_score.groupby("state")["fraud_score"].rank(pct=True)

df = fraud_score[fraud_score["percentile"] > 0.95]

result = df[["policy_num", "state", "claim_cost", "fraud_score"]]
```
Notes: <br/>
1. Calculate the percentile rank of the fraud scores within each state. <br/>
We can do this by grouping the data by the 'state' column and applying the `rank` function to the 'fraud_score' column with the argument `pct=True`. <br/>
This will give us the percentile rank of each fraud score, within its respective state. <br/>

2. Now we can filter the data to select only the claims with a percentile rank greater than 0.95
<br/>
