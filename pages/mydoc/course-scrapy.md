---
title: Web Scrapping
tags: 
keywords: rouge, pygments, prettify, color coding,
last_updated: Sept 5, 2022
summary: ""
sidebar: mydoc_sidebar
permalink: coursescrapy.html
folder: mydoc
---


[video course](https://learning.oreilly.com/videos/data-scraping-and/9781801818483/9781801818483-video4_19/) |
[Edit here](https://github.com/bhbharat/bhbharat.github.io/edit/gh-pages/pages/mydoc/course-scrapy.md)


## BeautifulSoup

```python
import requests
from bs4 import BeautifulSoup
page = requests.get("https://dataquestio.github.io/web-scraping-pages/simple.html")
print(page.status_code)

soup = BeautifulSoup(page.content, 'html.parser')
tag = soup.p
print(tag.attrs)
tag.get_attribute_list('id')
tag.find_all('div', attrs={'id':'all_quotes'})
tag.select("div p") # use css selector

```

## Selenium

```python

from selenium import webdriver
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.by import By
import time

def get_driver(download_dir='downloads'):
    options = Options()
    options.add_argument("--mute-audio")
    options.add_argument('--no-sandbox')
    options.add_argument('--disable-dev-shm-usage')
    options.add_argument("--incognito")
    options.add_argument("--window-size=900,500")
    options.add_experimental_option("prefs",{"download.default_directory" : f"{os.getcwd()+'/'+download_dir+'/'}"})
    driver = webdriver.Chrome(options=options, executable_path=chromedriver_path)
    return driver

def download_wait(path_to_downloads):
    seconds = 0
    dl_wait = True
    while dl_wait and seconds < 600:
        time.sleep(1)
        dl_wait = False
        for fname in os.listdir(path_to_downloads):
            if fname.endswith('.crdownload'):
                dl_wait = True
        seconds += 1
    return seconds
        
        
driver=get_driver()
driver.get("www.google.com")
time.sleep(5)

driver.find_element(By.CSS_SELECTOR, "#formAID" ).click()

email=driver.find_element(By.CSS_SELECTOR,"#mat-input-0")
email.send_keys('mebharat123@gmail.com')

from selenium.webdriver.support.ui import Select
select=Select(self.driver.find_element(By.CSS_SELECTOR,'#sprachwahl'))
select.select_by_visible_text("US English / Matthew")

driver.quit()

### Colab
!pip install selenium
!sudo apt install chromium-chromedriver
!cp /usr/lib/chromium-browser/chromedriver /usr/bin
chromedriver_path='/usr/bin/chromedriver'
options.add_argument('--headless')


```


## Snscrape for Twitter scraping

```python

import pandas as pd
import snscrape.modules.twitter as sntwitter
from tqdm import tqdm_notebook
# !snscrape --json --progress --max-results 100 twitter-search "from:jack" > user-tweets.json
# !snscrape --jsonl --progress --max-results 500 --since 2020-06-01 twitter-search "rafale until:2020-07-31" > text-query-tweets.json

term1=['term1a','term1b']
term2=['term2']
term3=['term3']
years=[2022,2021,2020,2019,2018,2017,2016,2015]

for year in years:
    for a in [term1,term2,term3]:
        tweets_list = []
        for b in tqdm_notebook(a):
            print(f'querying for {b}')
            for i,tweet in enumerate(sntwitter.TwitterSearchScraper(f'{b} near:delhi within:2000mi lang:en since:{year}-01-01 until:{year}-12-31').get_items()):
                tweets_list.append([tweet.date, tweet.id, tweet.content, tweet.user.username])

            tweets_df = pd.DataFrame(tweets_list, columns=['Datetime', 'Tweet Id', 'Text', 'Username'])
        tweets_df.to_csv(f'Tweets_{year}_{a[0]}.csv')
        
        
```




## CSS Selectors

1.  A - Selects all elements of type **A**. Type refers to the type of tag, so div, p and ul are all different element types.
 Examples - **div** selects all div elements.

2.  Selects the element with a specific **id**. You can also combine the ID selector with the type selector.
Examples - **#cool** selects any element with **id="cool"**

3.  A B - Selects all **B** inside of **A**. **B** is called a descendant because it is inside of another element.
Examples - **p strong** selects all strong elements that are inside of any p

4. #id A - You can combine any selector with the descendent selector.
Examples - **#cool span** selects all span elements that are inside of elements with **id="cool"**

5. .classname - The class selector selects all elements with that class attribute. Elements can only have one ID, but many classes.
Examples - **.neato** selects all elements with **class="neato"**

6. A.className - You can combine the class selector with other selectors, like the type selector.
Examples - **ul.important** selects all ul elements that have **class="important"**

7. A, B - Thanks to Shatner technology, this selects all **A** and **B** elements. You can combine any selectors this way, and you can specify more than two.
Examples - **p, .fun** selects all p elements as well as all elements with **class="fun"**

8. '*' - You can select all elements with the universal selector!
Examples - **p *** selects any element inside all p elements.

9. A * - This selects all elements inside of **A**.
Examples - **p *** selects every element inside all p elements.

10. A + B - This selects all **B** elements that directly follow **A**. Elements that follow one another are called siblings. They're on the same level, or depth. In the HTML markup for this level, elements that have the same indentation are siblings.
Examples - **p + .intro** selects every element with **class="intro"** that directly follows a p

11. A ~ B - You can select all siblings of an element that follow it. This is like the Adjacent Selector (A + B) except it gets all of the following elements instead of one.
 Examples - **A ~ B** selects all **B** that follow a **A**

12. A > B - You can select elements that are direct children of other elements. A child element is any element that is nested directly in another element. Elements that are nested deeper than that are called descendant elements.
Examples - **A > B** selects all **B** that are a direct children **A**

13. :first-child - You can select the first child element. A child element is any element that is directly nested in another element. You can combine this pseudo-selector with other selectors.
Examples - **:first-child** selects all first child elements.

14. :only-child - You can select any element that is the only element inside of another one.
Examples - **span:only-child** selects the span elements that are the only child of some other element.

15. :last-child - You can use this selector to select an element that is the last child element inside of another element.  
  Pro Tip → In cases where there is only one element, that element counts as the first-child, only-child and last-child!
Examples - **:last-child** selects all last-child elements. 

16. :nth-child(A) - Selects the **nth** (Ex: 1st, 3rd, 12th etc.) child element in another element. **:nth-child(8)** selects every element that is the 8th child of another element.

17.  nth-last-child(A) - Selects the children from the bottom of the parent. This is like nth-child, but counting from the back!
Examples - **:nth-last-child(2)** selects all second-to-last child elements.

18. :first-of-type - Selects the first element of that type within another element.
Examples - **span:first-of-type** selects the first span in any element.

19. :nth-of-type(A) - Selects a specific element based on its type and order in another element - or even or odd instances of that element.
Examples  - **div:nth-of-type(2)** selects the second instance of a div.

20. :nth-of-type(An+B) - The nth-of-type formula selects every nth element, starting the count at a specific instance of that element.
Examples - **span:nth-of-type(6n+2)** selects every 6th instance of a span, starting from (and including) the second instance.

21. :not(X) - You can use this to select all elements that do not match selector **"X"**.
 Examples-**:not(#fancy)** selects all elements that do not have **id="fancy"**.
 
22. [attribute] - Attributes appear inside the opening tag of an element, like this: span attribute="value". An attribute does not always have a value, it can be blank!
Examples - **a[href]** selects all a elements that have a **href="anything"** attribute.

23. A[attribute] - Combine the attribute selector with another selector (like the tag name selector) by adding it to the end.
Examples - **[value]** selects all elements that have a **value="anything"** attribute.

24. [attribute="value"] - Attribute selectors are case sensitive, each character must match exactly.
Examples - **input[type="checkbox"]** selects all checkbox input elements.

