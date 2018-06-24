---
title: "Collecting Streaming Twitter Data"
date: 2017-10-28T09:15:27-05:00
draft: false
index: true
tag: "tutorials"
---

Twitter no longer provides free access to all streaming tweets (Twitter Firehose). However, it does allow access to "some" streaming tweets in real time based on filters. In the code below all tweets having 'Las Vegas' in them will be filtered. [More: http://tweepy.readthedocs.io/en/v3.5.0/streaming_how_to.html]

One needs to be aware of the rate limits imposed by Twitter while accessing their feeds. It is always advisable to include pauses in code execution to ensure that the rate limits are not violated OR include error handling routines that can trap rate violation errors. (see Tweepy documentation at the above link)

Note that in the code below only the tweet text will be printed (and not the entire tweet). To print the entire tweet you will use \em{print(jdata)} instead of \em{print(jdata['text'])}. In addition, you might want to change the filter to suit the kind of tweets you want to collect.

```
#Filename: tstreaming.py

import tweepy
import json

access_token = ''
access_token_secret = ''
consumer_key = ''
consumer_secret = ''

class StdOutListener(tweepy.StreamListener) :
    def on_data(self, data) :
        try:
            jdata = json.loads(data)
            print(jdata['text'])
        except:
            pass
        return True
    def on_error(selfself, status):
        print(status)

if __name__ == "__main__" :
    listen = StdOutListener()
    auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
    auth.set_access_token(access_token, access_token_secret)
    stream = tweepy.Stream(auth, listen)
    stream.filter(track=['Las Vegas'])
```

# Saving collected data to disk
The output of the above Python program (`tstreaming.py`) can be saved to disk using the OS pipe filters. The following command executed at the OS command prompt will create a `tweets.txt` file with the streaming tweets. To terminate program execution one could use `Ctrl+C`. Let the above program execute for at least an hour so that a sufficient number of tweets are collected. (Note: The above program will execute without terminating. To terminate, press Ctrl+C.)
```
    python tstreaming.py > tweets.txt
```
