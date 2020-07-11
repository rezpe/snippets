# Streamlit Cheat Sheet

## Import
```python
import streamlit as st
```

## Data Cache
```python
@st.cache
def get_data():
  data = pd.read_csv("https://github.com/rezpe/datos_viz/blob/master/locales_madrid.csv?raw=true")
  return data
locales_df = get_data()
```

## Title
```python
st.title("Locales de Madrid")
```

## Markdown
```python
st.markdown("Hemos descargado la lista de locales de Madrid en el 2018. El mapa representa los bares")
```

## Tables

```python
# Static Table
st.table(sublocales_df.head())
# Orderable Table
st.dataframe(bares_df.head())
```

## Altair Charts

```python
bar_chart = alt.Chart(sublocales_df).mark_point().encode(
    latitude="lat",
    longitude="lon",
    color="desc_distrito_local:N"
)
st.write(bar_chart)
```

## Inputs

```python
st.button('Say hello')
agree = st.checkbox('I agree')
genre = st.radio(
     "What's your favorite movie genre",
     ('Comedy', 'Drama', 'Documentary'))
option = st.selectbox(
     'How would you like to be contacted?',
     ('Email', 'Home phone', 'Mobile phone'))
options = st.multiselect(
     'What are your favorite colors',
     ['Green', 'Yellow', 'Red', 'Blue'],
     ['Yellow', 'Red'])
age = st.slider('How old are you?', 0, 130, 25)

title = st.text_input('Movie title', 'Life of Brian')

number = st.number_input('Insert a number')

txt = st.text_area('Text to analyze', '''
     It was the best of times, it was the worst of times, it was
...     the age of wisdom, it was the age of foolishness, it was
...     the epoch of belief, it was the epoch of incredulity, it
...     was the season of Light, it was the season of Darkness, it
...     was the spring of hope, it was the winter of despair, (...)
...     ''')

import datetime
d = st.date_input(     "When's your birthday",
     datetime.date(2019, 7, 6))
t = st.time_input('Set an alarm for', datetime.time(8, 45))
uploaded_file = st.file_uploader("Choose a CSV file", type="csv")
