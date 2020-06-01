# Restaurant Response to COVID-19
The goal of this analysis is to examine sentiment surrounding restaurants following the impacts of the coronavirus outbreak in March, 2020. Across the U.S., restaurants have been forced to rapidly change their business strategies from dine-in to carry-out.

To gather data for this analysis, I scrape Yelp reviews for 200 restaurants in each of the 25 cities most impacted by COVID-19 (which I define as highest # of deaths instead of highest # of cases due to testing limitations, although this still is likely not the best way to measure this).

***
1. [Yelp Scraping](#Yelp-Scraping)  
    1a. [Identify COVID-19-Affected Counties](#Identify-COVID-19-Affected-Counties)  
    1b. [Identify Restaurants](#Identify-Restaurants)  
    1c. [Scrape Reviews](#Scrape-Reviews)  
2. [Restaurant Sentiment Analysis Around COVID-19](#Restaurant-Sentiment-Analysis-Around-COVID-19)  
    2a. [Sentiment Analysis](#Sentiment-Analysis)  

***
## [Yelp Scraping](yelp-scraping.ipynb)
The notebook for scraping Yelp can be found [here](yelp-scraping.ipynb). 

### Identify COVID-19-Affected Counties
First, I determined which cities were most impacted by COVID-19 using [data provided by the New York Times, available here](https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-counties.csv). This .csv is updated ~1x/day and contains number of COVID-19 cases and deaths by US county by day.

### Identify Restaurants
Using Yelp's API, I generated a list of up to 200 restaurants for each of the 25 counties. Each business has a unique ID that can be used to scrape individual reviews from the webpage. Of note, Yelp's API does allow you to access reviews, but only 3 per restaurant, and not the most recent 3: they are sorted using Yelp's sorting algorithm.

### Scrape Reviews
Using the business IDs I gathered using the API, I used BeautifulSoup to scrape up to 100 reviews per restaurant for each restaurant. This process was long and required attention throughout: Yelp's site detects the scraping and asks if you're a robot, at which point, the scraping stops. I kept a second Yelp window open and refreshed every 90 seconds; if I saw a message asking for verification, I stopped the scraping, verified my identity, and resumed. I looped back over the business IDs to re-scrape businesses that were missed several times. The scraping program takes approximately 1 hour per 200 businesses.

## Restaurant Sentiment Analysis Around COVID-19
The notebook for the sentiment analysis can be found here; I also uploaded an HTML version here, as the notebook contains linking throughout.

### Data Cleaning 
To prepare the review data for analysis, I used the spaCy library for text lemmatization and cleaning. Of note, there are two sepearate cleaning phases in this analysis, as VADER uses punctuation and capitalization to capture emphasis (see below).

### Sentiment Analysis
To complete my sentment analysis, I used [NTLK's VADER toolkit](https://github.com/cjhutto/vaderSentiment). VADER generates a positive, negative, and neutral score for each review and then combines them into a compound score ranging from -1 to 1. Throughout my analysis, I've examined the positive, negative, and compound scores, mainly to determine if shifts in compound scores are due to an increase in positive, negative, or neutral (if neither positive nor neutral) sentiment.

I broke the sentiment analysis down into pre- and post-lockdown using reviews left after March 15, 2020 as a cutoff date and examined differences between the two groups. Using bootstrap testing, I found a significant negative shift in the sentiment of post-lockdown reviews. I further examined sentiment by region and found that not all regions experienced a negative shift; however, New York City did experience this shift, in addition to ...  (will come back to this)

### Modeling of Reviews
Next, I generated a TF-IDF model from the reviews and used that to model the data using KMeans clustering. I determined that the discrete number of clusters was not clear, but there was a slight plateau at ~13 clusters. I used PCA to visualize the clusters in 2 dimensions. 

Finally, I searched the clusters for the maximum TI-IDF values for coronavirus-like terms and found they all grouped together, along with terms like "order", "time", "delivery", "support", "takeout", "service", and even "online". I revisited the sentiment analysis and determined that the sentiment of reviews within this cluster was significantly lower than the sentiment of the whole group, which might explain the decrease in overall sentiment after March 15, 2020.

***
_Note: plotly figures do not seem to be rendering using the github viewer for Jupyter Notebooks, even if you use nbviewer or view the html conversion of the notebook. I've uploaded htmls of the plotly figures into a seperate folder and compressed it - download that folder to view the interactive figures if you aren't downloading and running the notebook._ 


