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
