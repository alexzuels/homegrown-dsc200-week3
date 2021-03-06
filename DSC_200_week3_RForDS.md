---
title: "R for Data Science DSC 200 Week 3"
author: "Alex Zuelsdorf"
date: "December 10, 2018"
output: html_document
---
library(tidyverse)
#12.2.1 Question 2
Table2Cases <- filter(table2, type == "cases") %>%
  arrange(country, year)
Table2Cases
Table2Population <- filter(table2, type == "population") %>%
  arrange(country, year)
Table2Population
CasesPer10000 <- Table2Cases %>%
  mutate(population = Table2Population$count, Rate = (Table2Cases$count/population)*10000)
CasesPer10000 %>% 
  select(country,year,Rate)
#CasesPer10000 contains the TB rate but use select to isolate
View(table4a)
View(table4b)
#After viewing both we can combine through dividing 4a/4b to get rate
combined <- tibble(country = table4a$country, `1999` = table4a[["1999"]]/table4b[["1999"]]*10000, `2000` = table4a[["2000"]]/table4b[["2000"]]*10000)
combined
#The easiest to work with was 4a and 4b as we simply had to divide them

#Question 3
table2 %>% 
  filter(type == "cases") %>%
  ggplot(aes(year,count)) + 
  geom_line(aes(group = country), colour = "limegreen")

#12.3.3 Question 1
stocks <- tibble(
  year   = c(2015, 2015, 2016, 2016),
  half  = c(   1,    2,     1,    2),
  return = c(1.88, 0.59, 0.92, 0.17)
)
stocks %>% 
  spread(year, return) %>% 
  gather("year", "return", `2015`:`2016`)
#They are not symmetrical because of the way gather handles column information. For example the year is now a character vector in the table when running the code.

#Question 2
#table4a %>%
 #gather(1999, 2000, key = "year", value = "cases")
#This code fails because the years are wrong. Must have the `` around them.

table4a %>%
  gather(`1999`, `2000`, key = "year", value = "cases")

#Question 3
#Code fails because Phillip Woods has two age variables, we could add a column indicating whether the value is a unique or what instance it is of it

#Question 4
preg <- tribble(
  ~pregnant, ~male, ~female,
  "yes",     NA,    10,
  "no",      20,    12
)
tidypreg <- preg %>% 
  gather(male, female, key = "sex", value = "count")
tidypreg

#12.5.1 Question 1
#Fill Carries the previous value forward, spread sets a value to replace NA with, complete replaces with a list for all the NAs

#Question 2
#Determines to take the value above or below 
