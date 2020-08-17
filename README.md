# python-web-scraping
this is python weather scraping you can download the source code for free

import pandas as pd
import requests
from bs4 import BeautifulSoup

# selecting the website
page = requests.get(
    'https://forecast.weather.gov/MapClick.php?lat=39.68681000000004&lon=-104.90341999999998#.XzpdF-gzb4Y')
soup = BeautifulSoup(page.content, 'html.parser')
# select a part or whole element  to scrapping
week = soup.find(id="seven-day-forecast-body")

# selecting a whole elements of container
items = (week.find_all(class_="tombstone-container"))

# here we are getting all the items on it i.e{days,short description,temprature}
period_names = [item.find(class_='period-name').get_text() for item in items]
short_description = [item.find(class_='short-desc').get_text() for item in items]
temperatures = [item.find(class_='temp').get_text() for item in items]

# here we are creating the data tables like that
weather_stuff = pd.DataFrame(
    {'period': period_names,
     'short_descriptions': short_description,
     'temperature': temperatures,
     })
print(weather_stuff)

weather_stuff.to_csv('weather.csv')
