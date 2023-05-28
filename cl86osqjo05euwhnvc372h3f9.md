---
title: "API Scraping - E-Commerce Data - Excel & PostgreSQL Using Python"
seoTitle: "python  api-scraping"
datePublished: Sun Sep 18 2022 01:55:53 GMT+0000 (Coordinated Universal Time)
cuid: cl86osqjo05euwhnvc372h3f9
slug: api-scraping-e-commerce-data-excel-postgresql-using-python
canonical: https://dev.to/nothanii/api-scraping-e-commerce-data-excel-postgresql-using-python-12p9
cover: https://cdn.hashnode.com/res/hashnode/image/unsplash/OqOhYRjn_JY/upload/v1663465676430/O9YgPKM0r.jpeg
tags: api, python, ecommerce, scraping

---

Packages FOR This Process is
```
pip install jupyter
pip install requests
pip install pandas
pip install sqlalchemy
```
After Installation given Packages
We use THis E-Commerce Site for data
[https://www.currys.co.uk/](Link)
Now load this site and after That
Use this cmd On Chrome ANd inspect the Given page
ctrl+shift+s
And Click on Network
And after it Looks For File start with name navigation
and copy as a CURL By Right clicking on it

now go to google and search for
[https://www.onlinedevtools.in/curl](Link)
and paste the given file and convert It into Python
and AFter that Open Jupyter imports Given Libraries, which We installed earlier
```
import requests
import pandas as pd
import sqlalchemy
```
after that Paste the Given Code that we get from devtools site
Now, Create a JSON object and output data using this code
```
result_json  =  response.json()
result_json.keys()
listing =  result_json['resultsets']['default']['results']
```

After all, We Put everything together That the KEys WE HAve By running Upper Code


```
product_title = []
brand= []
price= []
review_score= []
review_count= []
description= []

for result in listing:
product_title.append(result['title'])
brand.append(result['brand'])
price.append(result['price'])
review_score.append(result['reevoo_score'])
review_count.append(result['reevoo_count'])
description.append(result['short_description'])
speakers_df = pd.DataFrame({'product_title':product_title,'brand':brand,'price':price,'review_score':review_score,
'review_count':review_count,'description':description})
speakers_df

```


after running all these cell you get Data from only One Page

So What we have t do if we need data from multiple pages

for that you have to make for loop
suppose i want data from 20 pages so we can us eloop that look like this
for i in range(1,21):
print (i)
Here the Complete Code for Multiple Pages
`
```
headers = {
'authority': 'api.cdn.dcg-search.com',
'sec-ch-ua': '^\^',
'sec-ch-ua-mobile': '?0',
'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.110 Safari/537.36',
'sec-ch-ua-platform': '^\^Windows^\^',
'accept': '/',
'origin': 'https://www.currys.co.uk',
'sec-fetch-site': 'cross-site',
'sec-fetch-mode': 'cors',
'sec-fetch-dest': 'empty',
'referer': 'https://www.currys.co.uk/',
'accept-language': 'en-US,en;q=0.9',
}
product_title = []
brand= []
price= []
review_score= []
review_count= []
description= []

for i in range(1,21):
params = (
('do', 'json'),
('m_results_per_page', '20'),
('page', str(i)),
('q', 'speaker^%%5E20docks'),
('sort', 'relevance'),
('sp_cs', 'UTF-8'),
)

response = requests.get('https://api.cdn.dcg-search.com/currys/navigation', headers=headers, params=params)

result_json  =  response.json()
listing =  result_json['resultsets']['default']['results']

for result in listing:
    #Product title
    try:
     product_title.append(result['title'])
    except:
        product_title.append('')

    #brand
    try:
        brand.append(result['brand'])
    except:
        brand.append('')
    try:

        price.append(float (result['price']))
    except:
        price.append('')
    try:
        review_score.append(float (result['reevoo_score']))
    except:
        review_score.append('')

    try:

        review_count.append(int (result['reevoo_count']))
    except:
        review_count.append('')
    try:

        description.append(result['short_description'])
    except:
        description.append('')
speakers_multiple_df = pd.DataFrame({'product_title':product_title,'brand':brand,'price':price,'review_score':review_score,
'review_count':review_count,'description':description})
speakers_multiple_df

`
```

after all we have to save data in excel file so for that we use this logic
```
speakers_multiple_df.to_excel('speakers_multiple.xlsx',index = False)
```
And also if we want to Save data and use in PostgreSQL
for that we use this logic For Multiple Pages
```
engine = sqlalchemy.create_engine('postgresql://postgres:"yourPassword"@localhost:5432')
#hAnii :(
speakers_multiple_df.to_sql('e-commerce',engine, index=False)
```
for getting this Data In PostgreSQL You HAve To Install this In Your Machine

That's It For Now. 