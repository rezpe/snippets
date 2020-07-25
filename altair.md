# Import altair

```python
import altair as alt
# if in notebook
alt.renderers.enable("notebook")
```

# Histogram

```python
alt.Chart(df).mark_bar().encode(
    x=alt.X('Brain Weight',bin=True,title="Brain Weight Binned"),
    y="count()"
).properties(
    width=200,
    height=200,
    title="Histogram of Brain Weight"
)
``` 

# Scatter Plot 

```python
scatter = alt.Chart(df).mark_circle().encode(
    x="Body Weight",
    y="Brain Weight"
)
``` 
# Bar Chart
```python
trends_bar=alt.Chart(trends).mark_bar().encode(
    x="search_term",
    y="sum(value)",
    color="search_term"
)
```

# Line Chart
```python
trends_line=alt.Chart(trends).mark_line().encode(
    x=alt.X("date:T",timeUnit="yearmonth"),
    y="sum(value)",
    color="search_term"
)
```

# Trail

```python
troops_viz = alt.Chart(troops).mark_trail(
    strokeCap="square"
).encode(
    latitude="lat",
    longitude="lon",
    color=alt.Color("direction",scale=alt.Scale(domain=['A', 'R'],range=["#EBD2A8","#888888"])),
    size=alt.Size("survivors",scale=alt.Scale(range=[1,75])),
    detail="division"
).properties(
    width=800
)
```

# Map

# Horizontal concatenation

```python
trends_bar|trends_line
```

# Vertical concatenation

```python
trends_bar&trends_line
```

# Single selection

```python
select_search_term = alt.selection_single(encodings=["x"])

trends_line=alt.Chart(trends).mark_line().encode(
    x=alt.X("date:T",timeUnit="yearmonth"),
    y="sum(value)",
    color="search_term"
).transform_filter(
    select_search_term
)

trends_bar=alt.Chart(trends).mark_bar().encode(
    x="search_term",
    y="sum(value)",
    color="search_term"
).properties(
    selection=select_search_term
)

trends_bar|trends_line
```

# Interval Selection

```python
selection = alt.selection_interval(encodings=["x"])

trends_line=alt.Chart(trends).mark_line().encode(
    x=alt.X("date:T",timeUnit="yearmonth"),
    y="sum(value)",
    color="search_term"
).transform_filter(
    selection
)

trends_line_small=alt.Chart(trends).mark_line().encode(
    x=alt.X("date:T",timeUnit="yearmonth"),
    y="sum(value)",
    color="search_term"
).properties(
    height=50,
    selection=selection
)

(trends_line&trends_line_small)
```
