# POI-Recommender-Service

This is a Recommender System that uses a subset of yelp dataset to perform a Top-N recomendation based on the interest you specified in a query sent in a JSON payload that the caller sent via the http post method.

This jupyter notebook performs the followings:

* a csv version of the dataset that can be obtained from https://www.kaggle.com/yelp-dataset/yelp-dataset
* read in the raw data and extracted the top rated (stars >= 4) businesses and their reviews  found in the dataset from two target cities: Edinburgh and Vegas. (this can be changed according to your need)
* vectorized the review text from those reviews into TF-IDF feature vectors
* trained up a Multinomial Naive Bayes classifier for each city
* exposes a REST endpoint to accept a query using the kernel gateway service

The REST endpoint has one resource: /recommend/{city_id} where city_id is the following:

city_id | City
------- | ----
1|Edinburgh
2|Las Vegas

The REST endpoint resource accepts a POST payload in the form of:
```javascript
{
  "query": "<Your query String>"
}
```

Examples 1: A free form query:
```javascript
{
  "query": "I want to do some shopping and visit must see landmarks"
}
```

Example 2: Just Keywords:
```javascript
{
  "query": "shopping, must see, landmarks"
}
```
If you POST this JSON to /recommend/1 you will get the top 10 recommendations from Edinburgh:

```javascript
[
    {
        "business_id": "DEVnhMGJni2cK5ZUQ3q_qA",
        "score": 0.0779863833,
        "name": "Edinburgh Castle",
        "address": "Edinburgh Castle, Castle Hill, EH1 2HG, Edinburgh",
        "postal_code": "EH1 2HG",
        "city": "Edinburgh",
        "categories": "Museums;Arts & Entertainment;Public Services & Government;Landmarks & Historical Buildings"
    },
    {
        "business_id": "XFWzenCibMgwvLsOdAkNxg",
        "score": 0.0352795544,
        "name": "Arthur's Seat",
        "address": "Arthur's Seat, EH8 8AZ, EH8 8AZ, Edinburgh",
        "postal_code": "EH8 8AZ",
        "city": "Edinburgh",
        "categories": "Hiking;Parks;Active Life;Public Services & Government;Landmarks & Historical Buildings"
    },
    {
        "business_id": "p9qyijcuXP6HKAFaVXl1Lg",
        "score": 0.0330101093,
        "name": "National Museum of Scotland",
        "address": "National Museum of Scotland, Chambers Street, EH1 1JF, Edinburgh",
        "postal_code": "EH1 1JF",
        "city": "Edinburgh",
        "categories": "Museums;Local Flavor;Arts & Entertainment"
    },
    {
        "business_id": "-F-hHAOj55_KGCh4-ZfnCg",
        "score": 0.0255828347,
        "name": "Princes Street",
        "address": "Princes Street, George Street, EH2 3AP, EH2 3AP, Edinburgh",
        "postal_code": "EH2 3AP",
        "city": "Edinburgh",
        "categories": "Landmarks & Historical Buildings;Local Flavor;Public Services & Government"
    },
    {
        "business_id": "XwR7hiCUj49db3OUNhJvuA",
        "score": 0.0251702084,
        "name": "Real Mary Kings Close",
        "address": "Real Mary Kings Close, 2 Warriston's Close, High Street, The Royal Mile, EH1 1PG, Edinburgh",
        "postal_code": "EH1 1PG",
        "city": "Edinburgh",
        "categories": "Local Flavor;Landmarks & Historical Buildings;Public Services & Government"
    },
    {
        "business_id": "ldSWESU5qI9DEvvYZUn4Ag",
        "score": 0.0245512688,
        "name": "Royal Botanic Garden Edinburgh",
        "address": "Royal Botanic Garden Edinburgh, Arboretum Place, 20A Inverleith Row, EH3 5LR, Edinburgh",
        "postal_code": "EH3 5LR",
        "city": "Edinburgh",
        "categories": "Active Life;Botanical Gardens;Parks;Arts & Entertainment"
    },
    {
        "business_id": "EkcvKpMSuaH_dbBgZ4TC3g",
        "score": 0.0229007634,
        "name": "Calton Hill",
        "address": "Calton Hill, St Andrews House, 2 Regent Road, EH1 3DG, Edinburgh",
        "postal_code": "EH1 3DG",
        "city": "Edinburgh",
        "categories": "Hotels & Travel;Travel Agents;Beer Tours;Landmarks & Historical Buildings;Travel Services;Local Flavor;Public Services & Government;Tours;Arts & Entertainment;Historical Tours;Festivals"
    },
    {
        "business_id": "quiMxKxKiA67WG-TIaAmhw",
        "score": 0.0204250052,
        "name": "Royal Mile",
        "address": "Royal Mile, Royal Mile, EH1 2PB, Edinburgh",
        "postal_code": "EH1 2PB",
        "city": "Edinburgh",
        "categories": "Beer Tours;Public Services & Government;Local Flavor;Tours;Restaurants;Shopping;Landmarks & Historical Buildings;Hotels & Travel"
    },
    {
        "business_id": "WqNx3bjn5ztcDVFQ8LT5qw",
        "score": 0.0167113679,
        "name": "The Meadows",
        "address": "The Meadows, Melville Dr, EH9 1ND, Edinburgh",
        "postal_code": "EH9 1ND",
        "city": "Edinburgh",
        "categories": "Active Life;Landmarks & Historical Buildings;Home & Garden;Nurseries & Gardening;Public Services & Government;Shopping;Parks;Local Flavor"
    },
    {
        "business_id": "kL3F9dg0fu9rv3If_aUjDA",
        "score": 0.0142356097,
        "name": "Camera Obscura and World of Illusions",
        "address": "Camera Obscura and World of Illusions, Castlehill, The Royal Mile, EH1 2ND, Edinburgh",
        "postal_code": "EH1 2ND",
        "city": "Edinburgh",
        "categories": "Museums;Local Flavor;Shopping;Arts & Entertainment;Art Galleries"
    }
]

```
