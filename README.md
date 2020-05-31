# Restaurant Response to COVID-19
In this analysis, I scrape Yelp reviews for 200 restaurants in each of the 25 cities most impacted by COVID-19 (which I define as highest # of deaths instead of highest # of cases due to testing limitations, although this still is likely not the best way to measure this).

1. [Yelp Scraping](#Yelp-Scraping)  
    1a. [Identify COVID-19-Affected Counties](#Identify-COVID-19-Affected-Counties)  
    1b. [Identify Restaurants](#Identify-Restaurants)  
    1c. [Scrape Reviews](#Scrape-Reviews)  

## [Yelp Scraping](yelp-scraping.ipynb)
The notebook for scraping Yelp can be found [here](yelp-scraping.ipynb). 

### Identify COVID-19-Affected Counties
First, I determined which cities were most impacted by COVID-19 using [data provided by the New York Times, available here](https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-counties.csv). This .csv is updated ~1x/day and contains number of COVID-19 cases and deaths by US county by day.

### Identify Restaurants
Using Yelp's API, I generated a list of up to 200 restaurants for each of the 25 counties. Each business has a unique ID that can be used to scrape individual reviews from the webpage. Of note, Yelp's API does allow you to access reviews, but only 3 per restaurant, and not the most recent 3: they are sorted using Yelp's sorting algorithm.

### Scrape Reviews
Using the business IDs I gathered using the API, I used BeautifulSoup to scrape up to 100 reviews per restaurant for each restaurant. This process was long and required attention throughout: Yelp's site detects the scraping and asks if you're a robot, at which point, the scraping stops. I kept a second Yelp window open and refreshed every 90 seconds; if I saw a message asking for verification, I stopped the scraping, verified my identity, and resumed. I looped back over the business IDs to re-scrape businesses that were missed several times. The scraping program takes approximately 1 hour per 200 businesses.

## Restaurant Sentiment Analysis Around COVID-19
The notebook for the sentiment analysis can be found here.

_Note: plotly figures do not seem to be rendering using the github viewer for Jupyter Notebooks, even if you use nbviewer or view the html conversion of the notebook. I've uploaded htmls of the plotly figures into a seperate folder and compressed it - download that folder to view the interactive figures if you aren't downloading and running the notebook._ 


