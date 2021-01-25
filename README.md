# rya_ebooks

this project should work in the latest releases of Python 2.7 and Python 3. i use Python 3 because it's easier to debug lately, as 2.7 is EOL.
i assume you might have both python 2 and python 3 installed, like me. if so, replace any `python3` commands with whatever you use to invoke python 3.

## setup

i dont know anymore
how does this even work

1. clone this repo
2. create a twitter account that you will post to, or use one you already have. i dunno, im not your parent.
3. sign into https://dev.twitter.com/apps with the same account that you'll use to post to and then create an application. Make sure that your application has read and write permissions to make POST requests.
4. in `ebooks.py`, set `ENABLE_TWITTER_SOURCES` and `ENABLE_TWITTER_POSTING` to `True`. this is set to TRUE by default for ez use.
5. in `local_settings.py`, add the @ of the account you want to randomise from. to make your tweets go live, change the `DEBUG` variable to `False`. i also have this enabled by default for ez use.
6. the only Python requirements for this script are [python-twitter](https://github.com/bear/python-twitter), Mastodon.py, and BeautfulSoup; so like, i think you type `pip3 install -r requirements.txt` in command line. if you only have Python 3 installed, just type `pip`, not `pip3`
7. um, i think that's it, but if you want to add or change shit, keep reading below.

## Configuring

in `local_settings.py`, there's a few tweaks that i've made to maximise cool shit.

```
ODDS = 0
```

the bot did some random shit and wouldnt send tweets all the time. but obviously you want it to, otherwise why would you make one? so i have mine set to 0. the below is an explanation from the initial git that i copied.

At the beginning of each time the script fires, `guess = random.choice(range(ODDS))`. If `guess == 0`, then it proceeds. If your `ODDS = 8`, it should run one out of every 8 times, more or less. You can override it to make it more or less frequent.

```
ORDER = 2
```

The ORDER variable represents the Markov index, which is a measure of associativity in the generated Markov chains. this basically just means how fuckin lit you want your bot to seem.
2 is generally more incoherent and 3 or 4 is more lucid.
i use 2, but might even go down to 1 sometime soon.

basically 2 gives you a litty bot, and 4 is a man in a suit talking business.

### Additional sources

this bot was made to pull shit from twitter, but you can nab ya archive or use random text from another place saved in a document or whatevs.
the below is basically all from the initial README of this bot, you might be able to figure it out but i couldn't be fucked tbh.

#### Static Text
To use a local text file, set `STATIC_TEST = True` and specify the name of a text file containing comma-separated "tweets" as `TEST_SOURCE`.

#### Web Content
To scrape content from the web, set `SCRAPE_URL` to `True`. This bot makes use of the [`find_all()` method](https://www.crummy.com/software/BeautifulSoup/bs4/doc/#find-all) of Python's BeautfulSoup library. The implementation of this method requires the definition of three inputs in `local_settings.py`.

1. A list of URLs to scrape as `SRC_URL`.
2. A list, `WEB_CONTEXT`, of the [names](https://www.crummy.com/software/BeautifulSoup/bs4/doc/#id11) of the elements to extract from the corresponding URL. This can be "div", "h1" for level-one headings, "a" for links, etc. If you wish to search for more than one name for a single page, repeat the URL in the `SRC_URL` list for as many names as you wish to extract.
3. A list, `WEB_ATTRIBUTES` of dictionaries containing [attributes](https://www.crummy.com/software/BeautifulSoup/bs4/doc/#attrs) to filter by. For instance, to limit the search to divs of class "title", one would pass the directory: `{"class": "title"}`. Use an empty dictionary, `{}`, for any page and name for which you don't wish to specify attributes.

__Note:__ Web scraping is experimental and may give you unexpected results. Make sure to test the bot in debugging mode before publishing.

#### Twitter archive
To use tweets from a Twitter account you have access to, you can download your Twitter Archive by following the steps from [Twitter's Help Center](https://help.twitter.com/en/managing-your-account/how-to-download-your-twitter-archive).

note: this shit doesnt work in 2020, cause twitter fucking sucks a big dick. i think i managed to grab some tweets but i ended up abandoning this archive way because man, it's not worth the fucking headache.

1. Request your Twitter archive
2. Extract the CSV file and ensure it is named the same as the `TWITTER_ARCHIVE_NAME` in `local_settings.py`
3. In `local_settings.py`, retweets are ignored by default. If you want to include retweets in your corpus, change `IGNORE_RETWEETS` to `False`.
4. Update `TEST_SOURCE` and specify the name of the parsed Twitter archive
5. Once that is all set, run `twittereater.py` and it will automatically create a corpus file based on the `TEST_SOURCE` variable in `local_settings.py`

If you want to use the Twitter corpus to generate tweets, set `STATIC_TEST = True`


## Debugging

If you want to test the script or to debug the tweet generation, you can skip the random number generation and not publish the resulting tweets to Twitter.

First, adjust the `DEBUG` variable in `local_settings.py`.

```
DEBUG = True
```

After that, commit the change and `git push heroku master`. Then run the command `heroku run worker` on the command line and watch what happens.

If you want to avoid hitting the Twitter API and instead want to use a static text file, you can do that. First, create a text file containing a Python list of quote-wrapped tweets. Then set the `STATIC_TEST` variable to `True`. Finally, specify the name of text file using the `TEST_SOURCE` variable in `local_settings.py`

## ok bye

love from rya
