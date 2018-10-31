## Loading lib

```R
library(dplyr)
```

## Select fields

```R
# Select President, Date, and Approve from approval_polls
approval_polls %>% 
  select(President,Date,Approve) %>%
  head()
``` 

## Filter

```R
# Select the President, Date, and Approve columns and filter to observations where President is equal to "Trump"
approval_polls %>% 
  select(President,Date,Approve) %>%
  filter(President == "Trump")
``` 

## Group by and Summarize

```R
# Group the approval_polls dataset by president and summarise a mean of the Approve column 
approval_polls %>%
  group_by(President) %>%
  summarise(Approve = mean(Approve))
```

## Pull

```R 
# Extract, or "pull," the Approve column as a vector and save it to the object "TrumpApproval"
TrumpApproval <- approval_polls %>% 
  select(President,Date,Approve) %>%
  filter(President == "Trump") %>%
  pull()

# Take a mean of the TrumpApproval vector
mean(TrumpApproval)
```

## Parsing Dates

```R
# Load the lubridate package for wrangling dates
library(lubridate)

# Select the relevant columns from the approval_polls dataset and filter them for the Trump presidency
TrumpPolls <- approval_polls %>% 
  select(President,Date,Approve) %>%
  filter(President == "Trump")
  
# Use the months() and mdy() function to get the month of the day each poll was taken
# Group the dataset by month and summarize a mean of Trump's job approval by month
TrumpPolls %>% 
  mutate(Month = mdy(Date)) %>%
  group_by(Month) %>%
  summarise(Approve = mean(Approve))
``` 

## Moving Average

```R
# Save Donald Trump's approval polling to a separate object 
TrumpApproval <- approval_polls %>% 
  filter(President == "Trump") %>%
  mutate(Date = mdy(Date)) %>%
  arrange(Date) 

# use the rollmean() function from the zoo package to get a moving average of the last 10 polls
library(zoo)
TrumpApproval <- TrumpApproval %>%
  mutate(AvgApprove = rollmean(Approve, 10, na.pad=TRUE, align = "right"))
```

## Visualize in chart
```R
# Load ggplot2
library(ggplot2)

# Use ggplot to graph Trump's average approval over time
ggplot(data = TrumpApproval, aes(x=Date,y=AvgApprove)) + 
  geom_line()
  
# Create an moving average of each president's approval rating
AllApproval <- approval_polls %>%
  group_by(President) %>%
  mutate(AvgApprove = rollmean(Approve, 10, na.pad=TRUE, align = "right"))


# Graph an moving average of each president's approval rating
ggplot(data = AllApproval, aes(x=Days, y=AvgApprove, col=President)) + 
    geom_line()
```
