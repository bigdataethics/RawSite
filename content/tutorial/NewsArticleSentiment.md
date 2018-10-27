---
title: "Introduction to Identifying News Article Sentiment"
description: "Sentiment Analysis in Python"
date: 2018-06-15T23:38:36-05:00
draft: false
index: true
tag: "tutorial"
---
## Sentiment Analysis
Identifying the sentiment of a news article or tweet can be useful in many ways. For example, sentiment of news about stock or the economy or sentiment of tweets of a user regarding the stock market might correlate to movements in the stock market. These in turn can be used for personalizing recommendations or for predicting market sentiment. In this brief tutorial we will look at how news article sentiment can be guaged.

### Python Packages:
```
pandas
nltk
textblob
news-please
```

### Getting a News Article
We have already seen in another tutorial how tweets can be collected. In this example we will work with news articles. BeautifulSoup and Scrapy are two popular Python packages for scraping news web-sites and getting links to articles. Another popular Python package that helps collect news articles is 'news-please'.

### Step 1: get the news article (in this case we are providing the url for the news article). 

```python
import newsplease
article = newsplease.NewsPlease.from_url('http://money.cnn.com/2018/07/03/news/companies/fiat-us-sales-future/index.html?iid=SF_LN')
```

#### Print some attributes of the "article".
The article title.
```python
# Print the title of the article
print(article.title)
```
```
    Plunging Fiat sales leave its American future in doubt
```
The text of the article. (Note: It might be necessary to clean the text so that items like 'Related:' do not affect the overall sentiment analysis. In this example the extra-text has been retained.)

```python
# Print the text of the article
print(article.text)
```
```
    Hardly anyone in America wants to buy a Fiat.
    Fiat's US sales are down 44% this year. The brand has sold just a third as many cars in 2018 as it did during the first six months of 2014, Fiat's recent high-water mark. Americans bought far more Alfa Romeos than Fiats this year.
    "You look at sales and wonder why the brand is still here," said Rebecca Lindland, analyst for Cox Automotive.
    The company sells four models in America: The Fiat 500, the 500L and 500X, which are three different vehicles, and the Spider. All are subcompact cars, which are falling out of favor with American customers.
    As sales fall, Fiat Chrysler (FCAU) has pulled spending for its small car brand. The company made clear it wants to emphasize trucks and SUVs over sedans going forward. Fiat, Dodge and Chrysler brands are collectively slated to receive just 25% of of the company's investment spending -- the lion's share will go to Ram and Jeep.
    Related: Things don't look good for Dodge and Chrysler
    "Nobody is making much of any money on cars, especially small, cheap cars," said Michelle Krebs, analyst for AutoTrader.
    Fiat could face an additional hurdle: the Trump administration's threat to impose tariffs on imported cars, particularly on those from Europe. That could significantly raise the price of Fiats -- potentially the final straw for an automaker already moving in another direction.
    Related: Every US-made car is an import. That's bad news for automakers
    Fiat rescued Chrysler from bankruptcy in 2009. The Italian brand returned to the US market two years later, following a three-decade absence.
    The brand never took off in the United States. Without, much demand or commitment from the company, many dealers probably don't want to continue giving their own resources and floor space to selling Fiats.
    "There's not the demand for cars and there's not much profit in them, especially for dealers," said Krebs.
    But the dealers have paid the dealership rights, and made investments to sell the brand. The company's decision whether or not to kill the Fiat brand in the United States may come down to the dealership math.
    "It could all depend on how much it is to buy them out," said Lindland. "If you're sitting there as a dealer, you'll want some sort of reparations."
    Only 13 American dealerships sell Fiat as their only Fiat Chrysler brand. Most of Fiat's nearly 400 US dealerships also sell Chryslers, Dodges, Jeeps and Rams. About 100 sell Alfa Romeos.
```
Other attributes of the article can be viewed by calling the appropriate methods.

```python
# Checking what other attributes are available
print(dir(article))
```
```
    ['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'authors', 'date_download', 'date_modify', 'date_publish', 'description', 'filename', 'get_dict', 'image_url', 'language', 'localpath', 'source_domain', 'text', 'title', 'title_page', 'title_rss', 'url']
```
Print the name(s) of the authors of the article.

```python
# Print the names of the author(s) of the article
print(article.authors)
```
```
    ['Chris Isidore']
```
Print the lead paragraph of the article.

```python
# Print the first paragraph of the article
print(article.description)
```
```
    Fiat's US sales are down 44% so far this year and many question how long Fiat Chrysler will keep brand in the US.
```

### Step 2: Identifying the sentiment of the news article

#### We will first find the sentiment of the article using the Python package textblob. The 'sentiment' call returns two values: polarity in the range -1 to +1, and subjectivity in the range 0 to 1. Polarity value -1 indicates 'negative', 0 indicates 'neutral', +1 indicates 'positive' sentiment. Subjectivity value 0 indicates 'objective', 1 indicates 'subjective'.

```python
from textblob import TextBlob
article_text = TextBlob(article.text)
print(article_text.sentiment)
```
```
    Sentiment(polarity=0.08844850948509486, subjectivity=0.4478319783197831)
```
The above output indicates that the article is close to 'neutral' in sentiment. But is that true? Let find the sentiment using another approach.

#### We will use the LoughranMcDonald word library (https://sraf.nd.edu/textual-analysis/resources/#LM%20Sentiment%20Word%20Lists) to identify positive and negative words and then score the article. The LoughranMcDonald word library is a list of words tagged as 'positive', 'negative', 'uncertainty', 'litigious', 'modal', or 'constraining'. 

#### The two LoughranMcDonald word library source files are avalable at:

GenericLong StopWords: https://drive.google.com/file/d/0B4niqV00F3msSktONVhfaElXeEk/view?usp=sharing

Master Dictionary: https://drive.google.com/file/d/0B4niqV00F3msaFZGUEZNTGtBblU/view?usp=sharing


#### A simple sentiment analzer can be constructed as follows:
1. Collect all 'positive' words and all 'negative' words from the main word library into two separate lists.
2. Count how many words of the article/tweet are found in the 'positive' list and in the 'negative' list.
3. If the count of 'positive' words is greater than the 'negative' words, the sentiment is 'positive'. If the count of 'negative' words is greater than the 'positive' words, the sentiment is 'negative'.
4. Before the article/tweet is analzed, it might be good to clean it of punctuations, stopwords, etc.


#### Tokenize the words in the text and convert all words to lowercase. Load the stopwords and remove stopwords from the article text.

```python
import pandas as pd
from nltk.tokenize import WordPunctTokenizer as wpt

# Tokenize the words and convert to lowercase
words = wpt().tokenize(article.text)
words = [word.lower() for word in words]
```

Print the tokenized list. (Note: Numbers and punctuation marks have not been removed.)

```python
print(words)
```
```
    ['hardly', 'anyone', 'in', 'america', 'wants', 'to', 'buy', 'a', 'fiat', '.', 'fiat', "'", 's', 'us', 'sales', 'are', 'down', '44', '%', 'this', 'year', '.', 'the', 'brand', 'has', 'sold', 'just', 'a', 'third', 'as', 'many', 'cars', 'in', '2018', 'as', 'it', 'did', 'during', 'the', 'first', 'six', 'months', 'of', '2014', ',', 'fiat', "'", 's', 'recent', 'high', '-', 'water', 'mark', '.', 'americans', 'bought', 'far', 'more', 'alfa', 'romeos', 'than', 'fiats', 'this', 'year', '.', '"', 'you', 'look', 'at', 'sales', 'and', 'wonder', 'why', 'the', 'brand', 'is', 'still', 'here', ',"', 'said', 'rebecca', 'lindland', ',', 'analyst', 'for', 'cox', 'automotive', '.', 'the', 'company', 'sells', 'four', 'models', 'in', 'america', ':', 'the', 'fiat', '500', ',', 'the', '500l', 'and', '500x', ',', 'which', 'are', 'three', 'different', 'vehicles', ',', 'and', 'the', 'spider', '.', 'all', 'are', 'subcompact', 'cars', ',', 'which', 'are', 'falling', 'out', 'of', 'favor', 'with', 'american', 'customers', '.', 'as', 'sales', 'fall', ',', 'fiat', 'chrysler', '(', 'fcau', ')', 'has', 'pulled', 'spending', 'for', 'its', 'small', 'car', 'brand', '.', 'the', 'company', 'made', 'clear', 'it', 'wants', 'to', 'emphasize', 'trucks', 'and', 'suvs', 'over', 'sedans', 'going', 'forward', '.', 'fiat', ',', 'dodge', 'and', 'chrysler', 'brands', 'are', 'collectively', 'slated', 'to', 'receive', 'just', '25', '%', 'of', 'of', 'the', 'company', "'", 's', 'investment', 'spending', '--', 'the', 'lion', "'", 's', 'share', 'will', 'go', 'to', 'ram', 'and', 'jeep', '.', 'related', ':', 'things', 'don', "'", 't', 'look', 'good', 'for', 'dodge', 'and', 'chrysler', '"', 'nobody', 'is', 'making', 'much', 'of', 'any', 'money', 'on', 'cars', ',', 'especially', 'small', ',', 'cheap', 'cars', ',"', 'said', 'michelle', 'krebs', ',', 'analyst', 'for', 'autotrader', '.', 'fiat', 'could', 'face', 'an', 'additional', 'hurdle', ':', 'the', 'trump', 'administration', "'", 's', 'threat', 'to', 'impose', 'tariffs', 'on', 'imported', 'cars', ',', 'particularly', 'on', 'those', 'from', 'europe', '.', 'that', 'could', 'significantly', 'raise', 'the', 'price', 'of', 'fiats', '--', 'potentially', 'the', 'final', 'straw', 'for', 'an', 'automaker', 'already', 'moving', 'in', 'another', 'direction', '.', 'related', ':', 'every', 'us', '-', 'made', 'car', 'is', 'an', 'import', '.', 'that', "'", 's', 'bad', 'news', 'for', 'automakers', 'fiat', 'rescued', 'chrysler', 'from', 'bankruptcy', 'in', '2009', '.', 'the', 'italian', 'brand', 'returned', 'to', 'the', 'us', 'market', 'two', 'years', 'later', ',', 'following', 'a', 'three', '-', 'decade', 'absence', '.', 'the', 'brand', 'never', 'took', 'off', 'in', 'the', 'united', 'states', '.', 'without', ',', 'much', 'demand', 'or', 'commitment', 'from', 'the', 'company', ',', 'many', 'dealers', 'probably', 'don', "'", 't', 'want', 'to', 'continue', 'giving', 'their', 'own', 'resources', 'and', 'floor', 'space', 'to', 'selling', 'fiats', '.', '"', 'there', "'", 's', 'not', 'the', 'demand', 'for', 'cars', 'and', 'there', "'", 's', 'not', 'much', 'profit', 'in', 'them', ',', 'especially', 'for', 'dealers', ',"', 'said', 'krebs', '.', 'but', 'the', 'dealers', 'have', 'paid', 'the', 'dealership', 'rights', ',', 'and', 'made', 'investments', 'to', 'sell', 'the', 'brand', '.', 'the', 'company', "'", 's', 'decision', 'whether', 'or', 'not', 'to', 'kill', 'the', 'fiat', 'brand', 'in', 'the', 'united', 'states', 'may', 'come', 'down', 'to', 'the', 'dealership', 'math', '.', '"', 'it', 'could', 'all', 'depend', 'on', 'how', 'much', 'it', 'is', 'to', 'buy', 'them', 'out', ',"', 'said', 'lindland', '.', '"', 'if', 'you', "'", 're', 'sitting', 'there', 'as', 'a', 'dealer', ',', 'you', "'", 'll', 'want', 'some', 'sort', 'of', 'reparations', '."', 'only', '13', 'american', 'dealerships', 'sell', 'fiat', 'as', 'their', 'only', 'fiat', 'chrysler', 'brand', '.', 'most', 'of', 'fiat', "'", 's', 'nearly', '400', 'us', 'dealerships', 'also', 'sell', 'chryslers', ',', 'dodges', ',', 'jeeps', 'and', 'rams', '.', 'about', '100', 'sell', 'alfa', 'romeos', '.']
```

Load the LoughranMcDonald generic stopword list into a pandas dataframe and convert it into a list.

```python
#Load stopword lists
stopwords = pd.read_csv("https://drive.google.com/file/d/0B4niqV00F3msSktONVhfaElXeEk/view?usp=sharing",names=['Word'])
stop_list = stopwords['Word'].tolist()
```

Take a look at what the stopwords are.

```python
print(stop_list)
```
```
    ['a', "a's", 'able', 'about', 'above', 'according', 'accordingly', 'across', 'actually', 'after', 'afterwards', 'again', 'against', "ain't", 'all', 'allow', 'allows', 'almost', 'alone', 'along', 'already', 'also', 'although', 'always', 'am', 'among', 'amongst', 'an', 'and', 'another', 'any', 'anybody', 'anyhow', 'anyone', 'anything', 'anyway', 'anyways', 'anywhere', 'apart', 'appear', 'appreciate', 'appropriate', 'are', "aren't", 'around', 'as', 'aside', 'ask', 'asking', 'associated', 'at', 'available', 'away', 'awfully', 'b', 'be', 'became', 'because', 'become', 'becomes', 'becoming', 'been', 'before', 'beforehand', 'behind', 'being', 'believe', 'below', 'beside', 'besides', 'best', 'better', 'between', 'beyond', 'both', 'brief', 'but', 'by', 'c', "c'mon", "c's", 'came', 'can', "can't", 'cannot', 'cant', 'cause', 'causes', 'certain', 'certainly', 'changes', 'clearly', 'co', 'com', 'come', 'comes', 'concerning', 'consequently', 'consider', 'considering', 'contain', 'containing', 'contains', 'corresponding', 'could', "couldn't", 'course', 'currently', 'd', 'definitely', 'described', 'despite', 'did', "didn't", 'different', 'do', 'does', "doesn't", 'doing', "don't", 'done', 'down', 'downwards', 'during', 'e', 'each', 'edu', 'eg', 'eight', 'either', 'else', 'elsewhere', 'enough', 'entirely', 'especially', 'et', 'etc', 'even', 'ever', 'every', 'everybody', 'everyone', 'everything', 'everywhere', 'ex', 'exactly', 'example', 'except', 'f', 'far', 'few', 'fifth', 'first', 'five', 'followed', 'following', 'follows', 'for', 'former', 'formerly', 'forth', 'four', 'from', 'further', 'furthermore', 'g', 'get', 'gets', 'getting', 'given', 'gives', 'go', 'goes', 'going', 'gone', 'got', 'gotten', 'greetings', 'h', 'had', "hadn't", 'happens', 'hardly', 'has', "hasn't", 'have', "haven't", 'having', 'he', "he's", 'hello', 'help', 'hence', 'her', 'here', "here's", 'hereafter', 'hereby', 'herein', 'hereupon', 'hers', 'herself', 'hi', 'him', 'himself', 'his', 'hither', 'hopefully', 'how', 'howbeit', 'however', 'i', "i'd", "i'll", "i'm", "i've", 'ie', 'if', 'ignored', 'immediate', 'in', 'inasmuch', 'inc', 'indeed', 'indicate', 'indicated', 'indicates', 'inner', 'insofar', 'instead', 'into', 'inward', 'is', "isn't", 'it', "it'd", "it'll", "it's", 'its', 'itself', 'j', 'just', 'k', 'keep', 'keeps', 'kept', 'know', 'knows', 'known', 'l', 'last', 'lately', 'later', 'latter', 'latterly', 'least', 'less', 'lest', 'let', "let's", 'like', 'liked', 'likely', 'little', 'look', 'looking', 'looks', 'ltd', 'm', 'mainly', 'many', 'may', 'maybe', 'me', 'mean', 'meanwhile', 'merely', 'might', 'more', 'moreover', 'most', 'mostly', 'much', 'must', 'my', 'myself', 'n', 'name', 'namely', 'nd', 'near', 'nearly', 'necessary', 'need', 'needs', 'neither', 'never', 'nevertheless', 'new', 'next', 'nine', 'no', 'nobody', 'non', 'none', 'noone', 'nor', 'normally', 'not', 'nothing', 'novel', 'now', 'nowhere', 'o', 'obviously', 'of', 'off', 'often', 'oh', 'ok', 'okay', 'old', 'on', 'once', 'one', 'ones', 'only', 'onto', 'or', 'other', 'others', 'otherwise', 'ought', 'our', 'ours', 'ourselves', 'out', 'outside', 'over', 'overall', 'own', 'p', 'particular', 'particularly', 'per', 'perhaps', 'placed', 'please', 'plus', 'possible', 'presumably', 'probably', 'provides', 'q', 'que', 'quite', 'qv', 'r', 'rather', 'rd', 're', 'really', 'reasonably', 'regarding', 'regardless', 'regards', 'relatively', 'respectively', 'right', 's', 'said', 'same', 'saw', 'say', 'saying', 'says', 'second', 'secondly', 'see', 'seeing', 'seem', 'seemed', 'seeming', 'seems', 'seen', 'self', 'selves', 'sensible', 'sent', 'serious', 'seriously', 'seven', 'several', 'shall', 'she', 'should', "shouldn't", 'since', 'six', 'so', 'some', 'somebody', 'somehow', 'someone', 'something', 'sometime', 'sometimes', 'somewhat', 'somewhere', 'soon', 'sorry', 'specified', 'specify', 'specifying', 'still', 'sub', 'such', 'sup', 'sure', 't', "t's", 'take', 'taken', 'tell', 'tends', 'th', 'than', 'thank', 'thanks', 'thanx', 'that', "that's", 'thats', 'the', 'their', 'theirs', 'them', 'themselves', 'then', 'thence', 'there', "there's", 'thereafter', 'thereby', 'therefore', 'therein', 'theres', 'thereupon', 'these', 'they', "they'd", "they'll", "they're", "they've", 'think', 'third', 'this', 'thorough', 'thoroughly', 'those', 'though', 'three', 'through', 'throughout', 'thru', 'thus', 'to', 'together', 'too', 'took', 'toward', 'towards', 'tried', 'tries', 'truly', 'try', 'trying', 'twice', 'two', 'u', 'un', 'under', 'unfortunately', 'unless', 'unlikely', 'until', 'unto', 'up', 'upon', 'us', 'use', 'used', 'useful', 'uses', 'using', 'usually', 'uucp', 'v', 'value', 'various', 'very', 'via', 'viz', 'vs', 'w', 'want', 'wants', 'was', "wasn't", 'way', 'we', "we'd", "we'll", "we're", "we've", 'welcome', 'well', 'went', 'were', "weren't", 'what', "what's", 'whatever', 'when', 'whence', 'whenever', 'where', "where's", 'whereafter', 'whereas', 'whereby', 'wherein', 'whereupon', 'wherever', 'whether', 'which', 'while', 'whither', 'who', "who's", 'whoever', 'whole', 'whom', 'whose', 'why', 'will', 'willing', 'wish', 'with', 'within', 'without', "won't", 'wonder', 'would', 'would', "wouldn't", 'x', 'y', 'yes', 'yet', 'you', "you'd", "you'll", "you're", "you've", 'your', 'yours', 'yourself', 'yourselves', 'z', 'zero']
```

Remove the stopwords from the article text word list.

```python
# Remove the stopwords
words_new = [word for word in words if word not in stop_list]
```

Take a look at words from the article text that are retained. (Note: Some punctuation marks still remain. Text could be further cleaned.)
```python
print (words_new)
```
```
    ['america', 'buy', 'fiat', '.', 'fiat', "'", 'sales', '44', '%', 'year', '.', 'brand', 'sold', 'cars', '2018', 'months', '2014', ',', 'fiat', "'", 'recent', 'high', '-', 'water', 'mark', '.', 'americans', 'bought', 'alfa', 'romeos', 'fiats', 'year', '.', '"', 'sales', 'brand', ',"', 'rebecca', 'lindland', ',', 'analyst', 'cox', 'automotive', '.', 'company', 'sells', 'models', 'america', ':', 'fiat', '500', ',', '500l', '500x', ',', 'vehicles', ',', 'spider', '.', 'subcompact', 'cars', ',', 'falling', 'favor', 'american', 'customers', '.', 'sales', 'fall', ',', 'fiat', 'chrysler', '(', 'fcau', ')', 'pulled', 'spending', 'small', 'car', 'brand', '.', 'company', 'made', 'clear', 'emphasize', 'trucks', 'suvs', 'sedans', 'forward', '.', 'fiat', ',', 'dodge', 'chrysler', 'brands', 'collectively', 'slated', 'receive', '25', '%', 'company', "'", 'investment', 'spending', '--', 'lion', "'", 'share', 'ram', 'jeep', '.', 'related', ':', 'things', 'don', "'", 'good', 'dodge', 'chrysler', '"', 'making', 'money', 'cars', ',', 'small', ',', 'cheap', 'cars', ',"', 'michelle', 'krebs', ',', 'analyst', 'autotrader', '.', 'fiat', 'face', 'additional', 'hurdle', ':', 'trump', 'administration', "'", 'threat', 'impose', 'tariffs', 'imported', 'cars', ',', 'europe', '.', 'significantly', 'raise', 'price', 'fiats', '--', 'potentially', 'final', 'straw', 'automaker', 'moving', 'direction', '.', 'related', ':', '-', 'made', 'car', 'import', '.', "'", 'bad', 'news', 'automakers', 'fiat', 'rescued', 'chrysler', 'bankruptcy', '2009', '.', 'italian', 'brand', 'returned', 'market', 'years', ',', '-', 'decade', 'absence', '.', 'brand', 'united', 'states', '.', ',', 'demand', 'commitment', 'company', ',', 'dealers', 'don', "'", 'continue', 'giving', 'resources', 'floor', 'space', 'selling', 'fiats', '.', '"', "'", 'demand', 'cars', "'", 'profit', ',', 'dealers', ',"', 'krebs', '.', 'dealers', 'paid', 'dealership', 'rights', ',', 'made', 'investments', 'sell', 'brand', '.', 'company', "'", 'decision', 'kill', 'fiat', 'brand', 'united', 'states', 'dealership', 'math', '.', '"', 'depend', 'buy', ',"', 'lindland', '.', '"', "'", 'sitting', 'dealer', ',', "'", 'll', 'sort', 'reparations', '."', '13', 'american', 'dealerships', 'sell', 'fiat', 'fiat', 'chrysler', 'brand', '.', 'fiat', "'", '400', 'dealerships', 'sell', 'chryslers', ',', 'dodges', ',', 'jeeps', 'rams', '.', '100', 'sell', 'alfa', 'romeos', '.']
```

#### Load the LoughranMcDonald master word list into a pandas dataframe. The dataframe has the following columns: 
Word, Sequence Number, Word Count, Word Proportion, Average Proportion, Std Dev, Doc Count, Negative, Positive,    Uncertainty, Litigious, Constraining, Superfluous, Interesting, Modal, Irr_Verb, Harvard_IV, Syllables, Source.

We will collect words whose  'Positive' or 'Negative' values are greater than 0 into two separate lists and convert words in those lists into lowercase.

```python
#Load master dictionary
master = pd.read_csv("https://drive.google.com/file/d/0B4niqV00F3msaFZGUEZNTGtBblU/view?usp=sharing")

positive = master[master["Positive"]>0]
negative = master[master["Negative"]>0]

pos_words = positive["Word"].tolist()
neg_words = negative["Word"].tolist()

pos_words = [word.lower() for word in pos_words]
neg_words = [word.lower() for word in neg_words]
```

#### Retain only the 'positive' words and 'negative' words from the article text and print the count of each.
```python
words_new_pos = [word for word in words_new if word in pos_words]
words_new_neg = [word for word in words_new if word in neg_words]

print("Positive Score: ",len(words_new_pos),"Negative Score: ",len(words_new_neg))
```
```
    Positive Score:  1 Negative Score:  5
```
The above scores indicate that the article has a negative sentiment.

#### Improving the sentiment analyzer.
The above method counts the number of positive words and negative words in the article and uses the difference in counts to indicate sentiment. The method can be improved by taking into account other collections (like constraining or uncertainity) in the master word list. Another improvement could give higher weight to 'positive' or 'negative' words that occur in the first section of the article.
