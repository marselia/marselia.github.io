---
layout: post
title: How to make a Twitter Bot in Python
categories: [Programming]
tags: [Twitter, bot, python, tweepy, OSINT]

---

Creating a bot account could be interesting for collecting data from an user, making statistics reports by likes/hashtags/locations, getting audience or for account creation. If you are interested in the OSINT section, you can use some of the features that I will discuss later or you can simply use this powerful tool: [twint](https://github.com/twintproject/twint).

There are several pages where they explain in detail everything that can be implemented with a bot account or with the Twitter API. You can take a look at the following pages which are quite interesting: https://botwiki.org/resource/tutorial/how-to-make-a-twitter-bot-the-definitive-guide/ and if you're familiar with Python: https://blog.mollywhite.net/how-to-create-a-twitter-bot/.

To start with the project you need a [developer account/bot account](https://developer.twitter.com/) where you create your APP (here are the API keys).

Now we can create our bot, but we must take into account the limitations it offers: 
- https://developer.twitter.com/en/docs/tweets/timelines/guides/migration-guide 
- https://developer.twitter.com/en/docs/basics/rate-limits

Some interesting documentation to start with where we can see all the features we could add: 
- https://developer.twitter.com/en/docs/tweets/data-dictionary/overview/tweet-object 
- http://docs.tweepy.org/en/v3.5.0/api.html#tweepy-api-twitter-api-wrapper.

And as a guideline for your project you can use this [github](https://github.com/molly/twitterbot_framework).

At this point, we'd just have to see what you'd like to implement. I leave you with the authentication part and some basic examples:

- Authentication:
First of all our main script must have our authentication part in order to use the Twitter API.
```
import tweepy
from secrets import * # secrets.py where you can see the Consumer Token, Consumer Token Secret, Access Token, and Access Token Secret.

auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)
api = tweepy.API(auth)
```

Some examples:

- Write a tweet: 
```
api.update_status("This is a tweet")
```

- Send a tweet with picture: 
```
api.update_with_media(filepath)
```

- Send a tweet with several images:
```
images = (path+'\q.jpg',path+ '\qq.jpg')
media_ids = [api.media_upload(i).media_id_string for i in images]
api.update_status(status='This is a tweet', media_ids=media_ids)
```

- Send several tweets with an image and text
```
#You could also do it with glob.glob(path+"*.jpg")
files = [file for file in listdir(path) if isfile(join(path, file))]
for i in files:    
    api.update_with_media(path+i,status='This is a tweet')
```

- RT, FAV and RT with comment:
```
api.retweet(tweet.id)
api.create_favorite(tweet.id)
api.update_status(“I like this tweet!! https://twitter.com/"+tweet.author._json['screen_name']+"/status/"+tweet.id_str) #RT with comment        
```

- Get mentions and reply them:
```
for mention in tweepy.Cursor(api.mentions_timeline).items():
    print(mention.text)
    print(mention.user.screen_name)
    print(mention.id)
    api.update_status("Thanks!", mention.id) #It can also be used to respond to X tweet.
```

- Send DMs:
You can send a DM when someone new follows you.
```
users = tweepy.Cursor(api.followers, screen_name=username, count=200).items()
for user in users:    
    api.send_direct_message(user.id,text = "Thanks!")
```

- Search tweets with a special word:
See [Rules and Filtering](https://developer.twitter.com/en/docs/tweets/rules-and-filtering/overview/standard-operators).
```
new_search = "Hello+world OR beautiful+cats -filter:retweets"    
    tweets = tweepy.Cursor(api.search,
                   q=new_search,
                   result_type='popular',                   
                   since=today).items(1000)
# Option 2:
for tweet in api.search(q=new_search, count=count):
```

- Tweet information:
```
user=str(tweet.author._json['screen_name'])
location=str(tweet.author._json['location'])
rts=str(tweet.retweet_count)
favs=str(tweet.favorite_count)       
print(user+" with "+rts+"RTS and "+favs+"FAVS https://twitter.com/"+user+"/status/"+tweet.id_str)        

```

- Get account information:
```
user=api.get_user(screen_name = 'user')
description=user.description
location=user.location 
url=user.url
followers_count=user.followers_count
friends_count=user.friends_count
```

- Get tweets from an account:
You can save the latest tweet and keep saving tweets.
```
f = open("accountdump.csv", "w")
f.write('user,created_at,tweet,url,location\n')
tweets=api.user_timeline(screen_name=user,
                           count=200, include_rts=False,
                           exclude_replies=True)
try: 
    for tweet in tweets:
        f.write(user+','+str(status.created_at)+”,”+status.full_text.encode('utf-8'))+", https://twitter.com/"+user+"/status/"+status.id_str+',"' + '",'+str(status.author._json['location'])+'\n')
except BaseException as e:
      print(str(e))
      time.sleep(3)
```

- Get pictures from an account:
You can add it in the previous loop.
```
mediatw= set()

for status in tweet: 
   
    media = status.entities.get('media', [])
    if(len(media) > 0):
        mediatw.add(media[0]['media_url'])

for media_file in mediatw:
    wget.download(media_file)

```

- Get the followers' names from an account and follow them:
```
users = tweepy.Cursor(api.followers, screen_name=the_user, count=200).items()
for user in users:
    print(user.screen_name)
    api.create_friendship(user.screen_name)
```

- Get the account names that a user is following:
```
users = tweepy.Cursor(api.friends, screen_name=the_user, count=300).items()
for user in users:
    print(user.screen_name)
```
