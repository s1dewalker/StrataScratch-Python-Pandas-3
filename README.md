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

## [Monthly Percentage Difference](https://platform.stratascratch.com/coding/10319-monthly-percentage-difference?code_type=2)

```python
sf_transactions['year_month'] = sf_transactions['created_at'].dt.to_period('M')

grouped_df = sf_transactions.groupby('year_month', as_index = False).agg(revenue = ('value', 'sum'))

grouped_df['prev_revenue'] = grouped_df['revenue'].shift()

grouped_df['revenue_diff_pct'] = (grouped_df['revenue'] - grouped_df['prev_revenue'])*100 / grouped_df['prev_revenue']

grouped_df[['year_month', 'revenue_diff_pct']]
```
Notes: 
1. To get "YYYY-MM" we can use `.dt.to_period('M')`
2. To get previous time series value we use `.shift()`
<br/>

## [The Most Popular Client_Id Among Users Using Video and Voice Calls](https://platform.stratascratch.com/coding/2029-the-most-popular-client_id-among-users-using-video-and-voice-calls?code_type=2)

```python
events_list  = ['video call received', 'video call sent', 'voice call received', 'voice call sent']

fact_events["event_check"] = fact_events["event_type"].apply(
    lambda x: 1 if x in events_list else 0
)

fact_events["event_check_mean"] = fact_events.groupby("user_id")[ "event_check"].transform("mean")

# Unlike agg(), which reduces the grouped data (e.g., returning a summary), transform() keeps the original shape of the DataFrame.

c = fact_events["event_check_mean"] >= 0.5
result = (fact_events[c].groupby("client_id")["id"].count().reset_index())

# The mean of these binary values (1 and 0) can be interpreted as the percentage of times the event occurred out of all the events for that user.

c2 = result['id'] == result['id'].max()
result[c2][['client_id']]
```
Notes: 
1. Unlike agg(), which reduces the grouped data (e.g., returning a summary), `transform()` keeps the original shape of the DataFrame.
2. The mean of these binary values (1 and 0) can be interpreted as the percentage of times the event occurred out of all the events for that user.
<br/>
