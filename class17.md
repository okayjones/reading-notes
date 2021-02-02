# Web Scraping

[Source: How to Web Scrape with Python in 4 Minutes](https://towardsdatascience.com/how-to-web-scrape-with-python-in-4-minutes-bc49186a8460)

Automatically access and extract large amounts of data from a website

## Walkthrough

- Source: [http://web.mta.info/developers/turnstile.html](http://web.mta.info/developers/turnstile.html)
- Goal: Download each .txt files linked on the page

> Read through T&Cs of a site to determine legallity of scraping

### Inspect

- Identify the data/pattern you're looking for
- In this example, .txt files are inside an `<a>` tag

### Implementation

#### Libraries

```python
import requests
import urllib.request
import time
from bs4 import BeautifulSoup
```

#### Set URL variable with target

```python
url = 'http://web.mta.info/developers/turnstile.html'
response = requests.get(url)
```

> if access successful, you'll get a 200 response in output

#### Use BeautifulSoup library to parse

```python
soup = BeautifulSoup(response.text, “html.parser”)

# find all <a> tags
soup.findAll('a')

# returns ALL links, manually look where ours start (line 38)
# Saves first text file to link
one_a_tag = soup.findAll(‘a’)[38]
link = one_a_tag[‘href’]

# build the download url
download_url = 'http://web.mta.info/developers/'+ link
urllib.request.urlretrieve(download_url,'./'+link[link.find('/turnstile_')+1:])

#space out the calls
time.sleep(1)
```

### Full example

```python
# Import libraries
import requests
import urllib.request
import time
from bs4 import BeautifulSoup

# Set the URL you want to webscrape from
url = 'http://web.mta.info/developers/turnstile.html'

# Connect to the URL
response = requests.get(url)

# Parse HTML and save to BeautifulSoup object¶
soup = BeautifulSoup(response.text, "html.parser")

# To download the whole data set, let's do a for loop through all a tags
line_count = 1 #variable to track what line you are on
for one_a_tag in soup.findAll('a'):  #'a' tags are for links
    if line_count >= 36: #code for text files starts at line 36
        link = one_a_tag['href']
        download_url = 'http://web.mta.info/developers/'+ link
        urllib.request.urlretrieve(download_url,'./'+link[link.find('/turnstile_')+1:]) 
        time.sleep(1) #pause the code for a sec
    #add 1 for next line
    line_count +=1
```
