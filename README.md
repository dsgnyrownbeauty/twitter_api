# twitter_api

import requests
import json
import time

from requests_oauthlib import OAuth1

# authentication pieces
client_key    = "TxjbtjPAXFR7RXJO3CbHH6ron"
client_secret = "3US9htwN35ktnsLkA5p1k1qXgXz7cY3CNsUFUg6je0fx1gO9Sm"
token         = "760126970860634112-GWq25svIWmjXXNwrylY1jkVDeQq59AH"
token_secret  = "CoNMaCyL3STlcZDjhnQPppzENHbkCsjDScbQ98XrnkyxV"

# the base for all Twitter calls
base_twitter_url = "https://api.twitter.com/1.1/"

# setup authentication
oauth = OAuth1(client_key,client_secret,token,token_secret)


#
# Download Tweets from a user profile
#

def download_tweets(screen_name,number_of_tweets):
    
    api_url  = "%s/statuses/user_timeline.json?" % base_twitter_url
    api_url += "screen_name=%s&" % screen_name
    api_url += "count=%d" % number_of_tweets

    # send request to Twitter
    response = requests.get(api_url,auth=oauth)
    
    if response.status_code == 200:
        
        tweets = json.loads(response.content)
        
        return tweets
    

    return None


# get a list of Tweets
tweet_list = download_tweets("jms_dot_py",10)

if tweet_list is not None:
    
    # loop over each Tweet and print the date and text
    for tweet in tweet_list:
        
        print "%st%s" % (tweet['created_at'],tweet['text'])
        
else:
    
    print "[*] No Tweets retrieved."
