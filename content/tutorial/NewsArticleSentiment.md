---
title: "Introduction to Identifying News Article Sentiment"
description: "Sentiment Analysis in Python"
date: 2018-06-15T23:38:36-05:00
draft: false
index: true
tag: "tutorials"
---
### Sentiment Analysis
Identifying the sentiment of a news article or tweet can be useful in many ways. For example, sentiment of news about stock or the economy might correlate to movements in the stock market. Or sentiment of tweets of an user might correlate to the mood of the user. These in turn can be used for personalizing recommendations or for predicting market sentiment. In this brief tutorial we will look at how news article sentiment can be guaged.

### Python Packages:
```
hello
```

### Getting a News Article
We have already seen how tweets can be collected. In this example we will work with news articles. BeautifulSoup and Scrapy are two popular Python packages for scraping news web-sites and getting links to articles. Another popular Python package that helps collect news articles is 'news-please'.

## Step 1: get the news article (in this case we are providing the url for the news article). 

### Websites can be scraped for news article links and those can be used for the actual analysis.
### "newsplease" is a Python package that enables extracting news articles.


```python
import newsplease
```


```python
article = newsplease.NewsPlease.from_url('https://www.cnn.com/2018/06/21/us/undocumented-migrant-children-detention-facilities-abuse-invs/index.html')
```


```python
print(article.title)
```

    Children allege grave abuse at migrant detention facilities
    


```python
print(article.text)
```

    (CNN) A year before the Trump administration's "zero-tolerance" policy resulted in more than 2,300 children being separated from their families at the border in a mere five-week period, a ninth-grader in McAllen, Texas, was taken from his mother.
    He was riding in a car with friends last spring when the car was pulled over. The teenager, brought illegally to the country by his mother as a baby, was unable to show identification. Police called immigration officials, who arrested the boy and sent him to a shelter for unaccompanied migrant children.
    John Doe 2, as he is referred to in current legal filings challenging his detainment, became one of thousands caught in a network of shelters and higher-security facilities that house undocumented minors, now gaining attention as newly separated children have been streaming in.
    Immigration attorneys working directly with migrant children say some of these facilities provide the best care they can, given the circumstances. And a huge shelter in Brownsville, Texas, which opened its doors for a media tour last week, appeared to be clean and well-staffed at the time.
    But John Doe 2 landed in a far more troubled corner of the system, according to a first-person sworn declaration in a current legal motion against the federal government for unlawful and inappropriate detainment of children. His account is one of dozens describing overloaded and secretive shelters, treatment centers and secure detention facilities for undocumented minors, which at their worst have allegedly been home to neglect, assault and other horrific abuse.
    


```python
# Checking what other attributes are available
print(dir(article))
```

    ['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'authors', 'date_download', 'date_modify', 'date_publish', 'description', 'filename', 'get_dict', 'image_url', 'language', 'localpath', 'source_domain', 'text', 'title', 'title_page', 'title_rss', 'url']
    


```python
print(article.authors)
```

    ['Blake Ellis', 'Melanie Hicken', 'Bob Ortega', 'Cnn Investigates']
    


```python
print(article.description)
```

    In court documents, children allege grave abuse at migrant detention facilities, ranging from handcuffs to assaults to drugs disguised as vitamins.
    

## Step 2: Identifying the sentiment of the news article

### WordTokenize the text; remove stopwords
### Use the LoughranMcDonald word library (https://sraf.nd.edu/textual-analysis/resources/#LM%20Sentiment%20Word%20Lists) to identify positive and negative words and then score the article.


```python
import pandas as pd
from nltk.tokenize import WordPunctTokenizer as wpt
```


```python
# Tokenize the words and convert to lowercase
words = wpt().tokenize(article.text)
words = [word.lower() for word in words]
```


```python
print(words)
```

    ['(', 'cnn', ')', 'a', 'year', 'before', 'the', 'trump', 'administration', "'", 's', '"', 'zero', '-', 'tolerance', '"', 'policy', 'resulted', 'in', 'more', 'than', '2', ',', '300', 'children', 'being', 'separated', 'from', 'their', 'families', 'at', 'the', 'border', 'in', 'a', 'mere', 'five', '-', 'week', 'period', ',', 'a', 'ninth', '-', 'grader', 'in', 'mcallen', ',', 'texas', ',', 'was', 'taken', 'from', 'his', 'mother', '.', 'he', 'was', 'riding', 'in', 'a', 'car', 'with', 'friends', 'last', 'spring', 'when', 'the', 'car', 'was', 'pulled', 'over', '.', 'the', 'teenager', ',', 'brought', 'illegally', 'to', 'the', 'country', 'by', 'his', 'mother', 'as', 'a', 'baby', ',', 'was', 'unable', 'to', 'show', 'identification', '.', 'police', 'called', 'immigration', 'officials', ',', 'who', 'arrested', 'the', 'boy', 'and', 'sent', 'him', 'to', 'a', 'shelter', 'for', 'unaccompanied', 'migrant', 'children', '.', 'john', 'doe', '2', ',', 'as', 'he', 'is', 'referred', 'to', 'in', 'current', 'legal', 'filings', 'challenging', 'his', 'detainment', ',', 'became', 'one', 'of', 'thousands', 'caught', 'in', 'a', 'network', 'of', 'shelters', 'and', 'higher', '-', 'security', 'facilities', 'that', 'house', 'undocumented', 'minors', ',', 'now', 'gaining', 'attention', 'as', 'newly', 'separated', 'children', 'have', 'been', 'streaming', 'in', '.', 'immigration', 'attorneys', 'working', 'directly', 'with', 'migrant', 'children', 'say', 'some', 'of', 'these', 'facilities', 'provide', 'the', 'best', 'care', 'they', 'can', ',', 'given', 'the', 'circumstances', '.', 'and', 'a', 'huge', 'shelter', 'in', 'brownsville', ',', 'texas', ',', 'which', 'opened', 'its', 'doors', 'for', 'a', 'media', 'tour', 'last', 'week', ',', 'appeared', 'to', 'be', 'clean', 'and', 'well', '-', 'staffed', 'at', 'the', 'time', '.', 'but', 'john', 'doe', '2', 'landed', 'in', 'a', 'far', 'more', 'troubled', 'corner', 'of', 'the', 'system', ',', 'according', 'to', 'a', 'first', '-', 'person', 'sworn', 'declaration', 'in', 'a', 'current', 'legal', 'motion', 'against', 'the', 'federal', 'government', 'for', 'unlawful', 'and', 'inappropriate', 'detainment', 'of', 'children', '.', 'his', 'account', 'is', 'one', 'of', 'dozens', 'describing', 'overloaded', 'and', 'secretive', 'shelters', ',', 'treatment', 'centers', 'and', 'secure', 'detention', 'facilities', 'for', 'undocumented', 'minors', ',', 'which',  'about', 'facilities', 'holding', 'migrant', 'children', '?', 'email', 'us', ':', 'watchdog', '@', 'cnn', '.', 'com', '.']
    


```python
#Load stopword lists
stopwords = pd.read_csv("StopWords_GenericLong.txt",names=['Word'])
stop_list = stopwords['Word'].tolist()
```


```python
print(stop_list)
```

    ['a', "a's", 'able', 'about', 'above', 'according', 'accordingly', 'across', 'actually', 'after', 'afterwards', 'again', 'against', "ain't", 'all', 'allow', 'allows', 'almost', 'alone', 'along', 'already', 'also', 'although', 'always', 'am', 'among', 'amongst', 'an', 'and', 'another', 'any', 'anybody', 'anyhow', 'anyone', 'anything', 'anyway', 'anyways', 'anywhere', 'apart', 'appear', 'appreciate', 'appropriate', 'are', "aren't", 'around', 'as', 'aside', 'ask', 'asking', 'associated', 'at', 'available', 'away', 'awfully', 'b', 'be', 'became', 'because', 'become', 'becomes', 'becoming', 'been', 'before', 'beforehand', 'behind', 'being', 'believe', 'below', 'beside', 'besides', 'best', 'better', 'between', 'beyond', 'both', 'brief', 'but', 'by', 'c', "c'mon", "c's", 'came', 'can', "can't", 'cannot', 'cant', 'cause', 'causes', 'certain', 'certainly', 'changes', 'clearly', 'co', 'com', 'come', 'comes', 'concerning', 'consequently', 'consider', 'considering', 'contain', 'containing', 'contains', 'corresponding', 'could', "couldn't", 'course', 'currently', 'd', 'definitely', 'described', 'despite', 'did', "didn't", 'different', 'do', 'does', "doesn't", 'doing', "don't", 'done', 'down', 'downwards', 'during', 'e', 'each', 'with', 'within', 'without', "won't", 'wonder', 'would', 'would', "wouldn't", 'x', 'y', 'yes', 'yet', 'you', "you'd", "you'll", "you're", "you've", 'your', 'yours', 'yourself', 'yourselves', 'z', 'zero']
    


```python
# Remove the stopwords
words_new = [word for word in words if word not in stop_list]
```


```python
print (words_new)
```

    ['(', 'cnn', ')', 'year', 'trump', 'administration', "'", '"', '-', 'tolerance', '"', 'policy', 'resulted', '2', ',', '300', 'children', 'separated', 'families', 'border', 'mere', '-', 'week', 'period', ',', 'ninth', '-', 'grader', 'mcallen', ',', 'texas', ',', 'mother', '.', 'riding', 'car', 'friends', 'spring', 'car', 'pulled', '.', 'teenager', ',', 'brought', 'illegally', 'country', 'mother', 'baby', ',', 'unable', 'show', 'identification', '.', 'police', 'called', 'immigration', 'officials', ',', 'arrested', 'boy', 'shelter', 'unaccompanied', 'migrant', 'children', '.', 'john', 'doe', '2', ',', 'referred', 'current', 'legal', 'filings', 'challenging', 'detainment', ',', 'thousands', 'caught', 'network', 'shelters', 'higher', '-', 'security', 'facilities', 'house', 'undocumented', 'minors', ',', 'gaining', 'attention', 'newly', 'separated', 'children', 'streaming', '.', 'immigration', 'attorneys', 'working', 'directly', 'migrant', 'children', 'facilities', 'provide', 'care', ',', 'circumstances', '.', 'huge', 'shelter', 'brownsville', ',', 'texas', ',', 'opened', 'doors', 'media', 'tour', 'week', ',', 'appeared', 'clean', '-', 'staffed', 'time', '.', 'john', 'doe', '2', 'landed', 'troubled', 'corner', 'system', ',', '-', 'person', 'sworn', 'declaration', 'current', 'legal', 'motion', 'federal', 'government', 'unlawful', 'inappropriate', 'detainment', 'children', '.', 'account', 'dozens', 'describing', 'overloaded', 'secretive', 'shelters', ',', 'treatment', 'ensured', 'lifelong', 'damage', 'children', '."', 'share', 'facilities', 'holding', 'migrant', 'children', '?', 'email', ':', 'watchdog', '@', 'cnn', '.', '.']
    


```python
#Load master dictionary
master = pd.read_csv("LoughranMcDonald_MasterDictionary_2016.csv")
```


```python
master.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Word</th>
      <th>Sequence Number</th>
      <th>Word Count</th>
      <th>Word Proportion</th>
      <th>Average Proportion</th>
      <th>Std Dev</th>
      <th>Doc Count</th>
      <th>Negative</th>
      <th>Positive</th>
      <th>Uncertainty</th>
      <th>Litigious</th>
      <th>Constraining</th>
      <th>Superfluous</th>
      <th>Interesting</th>
      <th>Modal</th>
      <th>Irr_Verb</th>
      <th>Harvard_IV</th>
      <th>Syllables</th>
      <th>Source</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AARDVARK</td>
      <td>1</td>
      <td>275</td>
      <td>1.603442e-08</td>
      <td>1.306189e-08</td>
      <td>3.665256e-06</td>
      <td>82</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>12of12inf</td>
    </tr>
    <tr>
      <th>1</th>
      <td>AARDVARKS</td>
      <td>2</td>
      <td>3</td>
      <td>1.749209e-10</td>
      <td>1.028197e-11</td>
      <td>1.014208e-08</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>12of12inf</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ABACI</td>
      <td>3</td>
      <td>8</td>
      <td>4.664558e-10</td>
      <td>1.465871e-10</td>
      <td>6.401309e-08</td>
      <td>7</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>3</td>
      <td>12of12inf</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ABACK</td>
      <td>4</td>
      <td>6</td>
      <td>3.498419e-10</td>
      <td>1.758203e-10</td>
      <td>7.213526e-08</td>
      <td>6</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>12of12inf</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ABACUS</td>
      <td>5</td>
      <td>6729</td>
      <td>3.923477e-07</td>
      <td>3.752169e-07</td>
      <td>3.452425e-05</td>
      <td>845</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>3</td>
      <td>12of12inf</td>
    </tr>
  </tbody>
</table>
</div>




```python
positive = master[master["Positive"]>0]
negative = master[master["Negative"]>0]
```


```python
pos_words = positive["Word"].tolist()
neg_words = negative["Word"].tolist()
```


```python
print(pos_words)
```

    ['ABLE', 'ABUNDANCE', 'ABUNDANT', 'ACCLAIMED', 'ACCOMPLISH', 'ACCOMPLISHED', 'ACCOMPLISHES', 'ACCOMPLISHING', 'ACCOMPLISHMENT', 'ACCOMPLISHMENTS', 'ACHIEVE', 'ACHIEVED', 'ACHIEVEMENT', 'ACHIEVEMENTS', 'ACHIEVES', 'ACHIEVING', 'ADEQUATELY', 'ADVANCEMENT', 'ADVANCEMENTS', 'ADVANCES', 'ADVANCING', 'ADVANTAGE', 'ADVANTAGED', 'ADVANTAGEOUS', 'ADVANTAGEOUSLY', 'ADVANTAGES', 'ALLIANCE', 'ALLIANCES', 'ASSURE', 'ASSURED', 'ASSURES', 'ASSURING', 'ATTAIN', 'ATTAINED', 'ATTAINING', 'ATTAINMENT', 'ATTAINMENTS', 'ATTAINS', 'ATTRACTIVE', 'ATTRACTIVENESS', 'BEAUTIFUL', 'BEAUTIFULLY', 'BENEFICIAL', 'BENEFICIALLY', 'BENEFIT', 'BENEFITED', 'BENEFITING', 'BENEFITTED', 'BENEFITTING', 'BEST', 'BETTER', 'BOLSTERED', 'BOLSTERING', 'BOLSTERS', 'BOOM', 'BOOMING', 'BOOST', 'BOOSTED', 'BREAKTHROUGH', 'BREAKTHROUGHS', 'BRILLIANT', 'CHARITABLE', 'COLLABORATE', 'COLLABORATED', 'COLLABORATES', 'COLLABORATING', 'COLLABORATION', 'COLLABORATIONS', 'COLLABORATIVE', 'COLLABORATOR', 'COLLABORATORS', 'COMPLIMENT', 'COMPLIMENTARY', 'COMPLIMENTED', 'COMPLIMENTING', 'COMPLIMENTS', 'CONCLUSIVE', 'CONCLUSIVELY', 'CONDUCIVE', 'CONFIDENT', 'CONSTRUCTIVE', 'CONSTRUCTIVELY', 'COURTEOUS', 'CREATIVE', 'CREATIVELY', 'CREATIVENESS', 'CREATIVITY', 'DELIGHT', 'DELIGHTED', 'DELIGHTFUL', 'DELIGHTFULLY', 'DELIGHTING', 'DELIGHTS', 'DEPENDABILITY', 'DEPENDABLE', 'DESIRABLE', 'DESIRED', 'DESPITE', 'DESTINED', 'DILIGENT', 'DILIGENTLY', 'DISTINCTION', 'DISTINCTIONS', 'DISTINCTIVE', 'DISTINCTIVELY', 'DISTINCTIVENESS', 'DREAM', 'EASIER', 'EASILY', 'EASY', 'EFFECTIVE', 'EFFICIENCIES', 'EFFICIENCY', 'UNSURPASSED', 'UPTURN', 'UPTURNS', 'VALUABLE', 'VERSATILE', 'VERSATILITY', 'VIBRANCY', 'VIBRANT', 'WIN', 'WINNER', 'WINNERS', 'WINNING', 'WORTHY']
    


```python
print(neg_words)
```

    ['ABANDON', 'ABANDONED', 'ABANDONING', 'ABANDONMENT', 'ABANDONMENTS', 'ABANDONS', 'ABDICATED', 'ABDICATES', 'ABDICATING', 'ABDICATION', 'ABDICATIONS', 'ABERRANT', 'ABERRATION', 'ABERRATIONAL', 'ABERRATIONS', 'ABETTING', 'ABNORMAL', 'ABNORMALITIES', 'ABNORMALITY', 'ABNORMALLY', 'ABOLISH', 'ABOLISHED', 'ABOLISHES', 'ABOLISHING', 'ABROGATE', 'ABROGATED', 'ABROGATES', 'ABROGATING', 'ABROGATION', 'ABROGATIONS', 'ABRUPT', 'ABRUPTLY', 'ABRUPTNESS', 'ABSENCE', 'ABSENCES', 'ABSENTEEISM', 'ABUSE', 'ABUSED', 'ABUSES', 'ABUSING', 'ABUSIVE', 'ABUSIVELY', 'ABUSIVENESS', 'ACCIDENT', 'ACCIDENTAL', 'ACCIDENTALLY', 'ACCIDENTS', 'ACCUSATION', 'ACCUSATIONS', 'ACCUSE', 'ACCUSED', 'ACCUSES', 'ACCUSING', 'ACQUIESCE', 'ACQUIESCED', 'ACQUIESCES', 'ACQUIESCING', 'ACQUIT', 'ACQUITS', 'ACQUITTAL', 'ACQUITTALS', 'ACQUITTED', 'ACQUITTING', 'ADULTERATE', 'ADULTERATED', 'ADULTERATING', 'ADULTERATION', 'ADULTERATIONS', 'ADVERSARIAL', 'ADVERSARIES', 'ADVERSARY', 'ADVERSE', 'ADVERSELY', 'ADVERSITIES', 'ADVERSITY', 'AFTERMATH', 'AFTERMATHS', 'AGAINST', 'AGGRAVATE', 'AGGRAVATED', 'AGGRAVATES', 'AGGRAVATING', 'AGGRAVATION', 'AGGRAVATIONS', 'ALERTED', 'ALERTING', 'ALIENATE', 'ALIENATED', 'ALIENATES', 'ALIENATING', 'ALIENATION', 'ALIENATIONS', 'ALLEGATION', 'ALLEGATIONS', 'ALLEGE', 'ALLEGED', 'ALLEGEDLY', 'ALLEGES', 'ALLEGING', 'ANNOY', 'ANNOYANCE', 'ANNOYANCES', 'ANNOYED', 'ANNOYING', 'ANNOYS', 'ANNUL', 'ANNULLED', 'ANNULLING', 'ANNULMENT', 'ANNULMENTS', 'ANNULS', 'ANOMALIES', 'ANOMALOUS', 'ANOMALOUSLY', 'ANOMALY', 'ANTICOMPETITIVE', 'ANTITRUST', 'ARGUE', 'ARGUED', 'ARGUING', 'ARGUMENT', 'ARGUMENTATIVE', 'ARGUMENTS', 'ARREARAGE', 'ARREARAGES', 'ARREARS', 'ARREST', 'ARRESTED', 'ARRESTS', 'ARTIFICIALLY', 'ASSAULT', 'ASSAULTED', 'ASSAULTING', 'ASSAULTS', 'ASSERTIONS', 'ATTRITION', 'AVERSELY', 'BACKDATING', 'BAD', 'BAIL', 'BAILOUT', 'BALK', 'BALKED', 'BANKRUPT', 'BANKRUPTCIES', 'BANKRUPTCY', 'BANKRUPTED', 'BANKRUPTING', 'BANKRUPTS', 'BANS', 'BARRED', 'BARRIER', 'BARRIERS', 'BOTTLENECK', 'BOTTLENECKS', 'BOYCOTT', 'BOYCOTTED', 'BOYCOTTING', 'WEAKNESSES', 'WILLFULLY', 'WORRIES', 'WORRY', 'WORRYING', 'WORSE', 'WORSEN', 'WORSENED', 'WORSENING', 'WORSENS', 'WORST', 'WORTHLESS', 'WRITEDOWN', 'WRITEDOWNS', 'WRITEOFF', 'WRITEOFFS', 'WRONG', 'WRONGDOING', 'WRONGDOINGS', 'WRONGFUL', 'WRONGFULLY', 'WRONGLY']
    


```python
pos_words = [word.lower() for word in pos_words]
neg_words = [word.lower() for word in neg_words]
```


```python
words_new_pos = [word for word in words_new if word in pos_words]
words_new_neg = [word for word in words_new if word in neg_words]
```


```python
print("Positive Score: ",len(words_new_pos),"Negative Score: ",len(words_new_neg))
```

    Positive Score:  7 Negative Score:  220
    


```python
## The above scores indicate that the given news article has a negative sentiment
```
