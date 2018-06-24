---
title: "Introduction to Reading Twitter Data in Python"
description: "Accessing Twitter data in Python"
date: 2017-10-27T23:38:36-05:00
draft: false
index: true
tag: "tutorials"
---
### Social Media Analysis
Social media has become the go-to place for information and opinions. Social media can be mined to gauge sentiment, analyze opinion, identity interests, locate concerns, and so on. Facebook, Pinterest, Instagram, Twitter, Google+ are some social media platforms. Among them, Twitter data is public and can be mined relatively easily.

### First of all you need to obtain access codes from Twitter to be able to mine Twitter data.
1. Go to www.apps.twitter.com
2. Sign in with your twitter credentials
3. Click on 'Create New App'
4. Give: (a) a name for the application; (b) a short description of what the app does; (c) a placeholder website address; (d) agree to the terms; and click 'Create Your Twitter Application'
5. Click on 'Keys and Access Tokens' tab. You will see that the Consumer key and Consumer secret are already generated. You will also need Access tokens. Click on 'Create my access token' to create access tokens.
6. Copy down your Consumer Key, Consumer Secret, Access Token, and Access Token Secret for use in the next section.

### Importing the required libraries
For most of the work in this section we will be using the following libraries. Install them in your Python installation using 'conda' or 'pip'.
1. tweepy
2. urllib3
3. textblob

### Getting access to Twitter in Python
This involves two steps:
1. Creating an authentication token to verify user permissions
2. Creating an API to access twitter data

Paste your consumer key, consumer secret, access token, and access token secret in the proper places in the code below.


```python
import tweepy

consumer_key = ''
consumer_secret = ''
access_token = ''
access_token_secret = ''

# Create authentication token
auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)

# Create API
api = tweepy.API(auth)
```

### We can now use the API to access twitter data
The code given below fetches some general information about the given user. Some of the user object attributes are: id, id_str, name, screen_name, location, url, description, protected, verified, followers_count, friends_count, listed_count, favorites_count, statuses_count, created_at, time_zone, geo_enabled, lang, etc.


```python
# Store a twitter handle in this variable (@username)
user = ''

# Retrieve information about the user
twitterData = api.get_user(user)

# Print the information
print('Twitter user       : ' + user)
print('Number of followers: ' + str(twitterData.followers_count))
print('Number of tweets   : ' + str(twitterData.statuses_count))
print('Favorites          : ' + str(twitterData.favourites_count))
print('Friends            : ' + str(twitterData.friends_count))
print('Appears on ' + str(twitterData.listed_count) + ' lists')
```

### Reading tweets of any user
Twitter allows us to read up to 3200 most recent tweets of any user. Tweets may be read in pages, each page can have up to 200 tweets. In all we can read up to 16 pages of 200 tweets each for any user. The Cursor method handles the page-wise fetching of data. The Tweepy API wrapper includes timeline methods, status methods, user methods, etc. [More: http://tweepy.readthedocs.io/en/v3.5.0/api.html#tweepy-api-twitter-api-wrapper]

In the code below we use the `user_timeline` method to obtain tweets from the user's timeline. We use the try-except construct for error handling.


```python
try:
    for page in tweepy.Cursor(api.user_timeline, screen_name=user, count=200).pages(16):
        for status in page:
            try:
                print(status.text)
                print('-----')
            except:
                print("Unable to print current status text")
                continue
except:
    print("Error retrieving user tweets")
    pass
```

### A Sample Tweet
A tweet consists of many attributes apart from the tweet text itself. In addition, a re-tweet will have additional attributes marked. Below is a sample tweet. You will notice that is has the key-value pair format and most of the keys are self explanatory.

Note some of the important keys:
1. Related to the user: user:{name, screen_name, location, description, followers_count, friends_count, listed_count, favourites_count, statuses_count, created_at, lang}
2. Related to the tweet: {created_at, text, geo, coordinates, place, retweet_count, favorite_count, entities:{hashtags, urls, user_mentions, symbols}, favorited, retweeted, lang

```
{"created_at":"Mon May 16 19:36:08 +0000 2016", "id":732293574721556480, "id_str":"732293574721556480", "text":"Le Petit March... all the ambience and spunk you'd want in a breakfast cafe! #goodfood #atlanta\u2026 https:\/\/t.co\/LFmuWcEaMO", "source":"\u003ca href=\"http:\/\/instagram.com\" rel=\"nofollow\"\u003eInstagram\u003c\/a\u003e", "truncated":false, "in_reply_to_status_id":null, "in_reply_to_status_id_str":null, "in_reply_to_user_id":null, "in_reply_to_user_id_str":null, "in_reply_to_screen_name":null, "user":{"id":21735586, "id_str":"21735586", "name":"Jessica Gallon", "screen_name":"JessicaGallon", "location":"USA", "url":"http:\/\/www.8PINTSdesign.com", "description":"Looking forward to looking forward. \n&emsp;\nGraphic Design \u2022 Photography \u2022  Life", "protected":false, "verified":false, "followers_count":157, "friends_count":68, "listed_count":38, "favourites_count":674, "statuses_count":2459, "created_at":"Tue Feb 24 06:46:56 +0000 2009", "utc_offset":-18000, "time_zone":"Central Time (US & Canada)", "geo_enabled":false, "lang":"en", "contributors_enabled":false, "is_translator":false, "profile_background_color":"B8B8B8", "profile_background_image_url":"http:\/\/pbs.twimg.com\/profile_background_images\/746770356 \/d11e6fa692d05af60009a12bf24d6d94.jpeg", "profile_background_image_url_https":"https:\/\/pbs.twimg.com\/profile_background_images \/746770356\/d11e6fa692d05af60009a12bf24d6d94.jpeg", "profile_background_tile":false, "profile_link_color":"A3A3A3", "profile_sidebar_border_color":"000000", "profile_sidebar_fill_color":"CFD6B8", "profile_text_color":"164706", "profile_use_background_image":true, "profile_image_url":"http:\/\/pbs.twimg.com\/profile_images\/696859863792668672\/h1-BJSY7_normal.jpg", "profile_image_url_https":"https:\/\/pbs.twimg.com\/profile_images\/696859863792668672\/h1-BJSY7_normal.jpg", "profile_banner_url":"https:\/\/pbs.twimg.com\/profile_banners\/21735586\/1454980052", "default_profile":false, "default_profile_image":false, "following":null, "follow_request_sent":null, "notifications":null}, "geo":null, "coordinates":null, "place":null, "contributors":null, "is_quote_status":false, "retweet_count":0, "favorite_count":0, "entities":{"hashtags":[{"text":"goodfood","indices":[77,86]},{"text":"atlanta","indices":[87,95]}], "urls":[{"url":"https:\/\/t.co\/LFmuWcEaMO", "expanded_url":"https:\/\/www.instagram.com\/p\/BFeubLSjo7X\/", "display_url":"instagram.com\/p\/BFeubLSjo7X\/","indices":[97,120]}], "user_mentions":[],"symbols":[]}, "favorited":false, "retweeted":false, "possibly_sensitive":false,"filter_level":"low", "lang":"en", "timestamp_ms":"1463427368227"}
```
