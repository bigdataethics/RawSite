+++
draft = false
tag = "tutorial"
description = "Dictionaries in Python"
title = "Dictionaries in Python for Textual Data"
highlight = true
css = []
scripts = []
index = true

date = "2018-10-26T10:00:00"

+++

Python provides a powerful natural language processing toolkit for handling textual data. The toolkit has functions for operations like tokenizing as well as parts-of-speech identification, n-gramming, etc. In addition it contains a corpora of useful datasets like 'stopwords', 'synonyms', etc.

#### Natural Language ToolKit (NLTK)

To **install the NLTK library**, run the following at the system command prompt (terminal window): *You will have to change your current directory to the 'Scripts' folder of the 'Python' directory OR you should have the Python folder in the system path variable. (The `pip` command file is in the Scripts folder).*

```
pip install nltk
```

To **download the NLTK Corpora**, run the following statements at the Python Prompt:

```python
import nltk
nltk.download()
```

Apart from NLTK, there are other toolkits like TextBlob, StandfordNLP, spaCy, etc. that can be used for natural language processing. Many of them are built upon NLTK. You can install any of those libraries using `pip`.

#### Basic Natural Language Processing

One of the basic operations to do with text is **tokenizing**. And two kinds of tokenizing are: sentence tokenizing and word tokenizing. Sentence tokenizing refers to splitting a paragraph into sentences; while word tokenizing refers to splitting a paragraph into words.

The paragraph shown below is taken from the link: [I tried a #NoSpendChallenge for a month](https://www.bbc.co.uk/bbcthree/article/d0438e75-f9fc-4a85-a5b4-ce3cc040e33d) (*Note: You will want to change the smart quotes to regular quotes*.)

```python
paragraph = "It's official: I'm a financial ostrich. Every month, I run \
out of money and can't work out where it's all gone. When an unexpected bill \
lands on my door mat (hello, tax return!) I'm hit with a cold panic and, \
even though I have one, my savings account is empty."
```

Let's see what sentence tokenizing and word tokenizing do with this paragraph. The tokenize functions are part of the nltk.tokenize section of nltk. Instead of importing the entire nltk we can import just the section or object we want. To import just the tokenize section use `import nltk.tokenize` (or to refer to it with a shortened name use `import nltk.tokenize as nt`). To import just the object use:

```
from nltk.tokenize import sent_tokenize
```

```python
print(sent_tokenize(paragraph))
```

Notice that sent_tokenize creates a list of sentences. It performs well with regular sentences. Did it do a good job with the paragraph? If not, what was the reason?

There are at least two ways of doing word tokenization. The first is using the object `word_tokenize` and the second is using the object `WordPunctTokenizer`. Try out this code and identify the differences between the way words are tokenized.

```python
from nltk.tokenize import word_tokenize, WordPunctTokenizer
print(word_tokenize(paragraph))
wpt = WordPunctTokenizer()
print(wpt.tokenize(paragraph))
```

What is the difference between using `.split()` and using `word_tokenize`or `WordPunctTokenizer`? Try them out with different texts and see which one works best for the text you want to process.

Another basic operation on text is **n-gramming**, that is creating a list of tuples of n-words of the text, where n can be 'bi' (2), or 'tri' (3), or any other number. For the  given paragraph, the list of bi-grams would be: `[("It's",'official'),('official',"I'm"),("I'm",'a'),('a','financial'),.....]`.

As always, first import the object (in this case: `ngrams`). Then create a list of items to n-gram (in this case: a list of words from the given text). (*Remember: for strings, the items are characters. So n-gramming a string will result in a list of n-characters as items*.)

```python
from nltk import ngrams
import string
words = [word.strip(string.punctuation) for word in paragraph.split()]
bigrams = ngrams(words, 2)
print(list(bigrams))
```

Try n-gramming with `paragraph` instead of `words`. What happens? How can tri-grams be created? Apart from the generic `ngrams` object, NLTK also has objects called `bigrams` and `trigrams` which take only one parameter, the word list.

Not all words in a text are equally important. Words that are most common in a language and not "essential" to the general meaning of the text are called **stopwords**. There is no standard definition of stopwords and neither is there is standard set of stopwords. Different NLP tools have their own set of stopwords. NLTK contains `stopwords` for different languages.

```python
from nltk.corpus import stopwords
englishstopwords = set(stopwords.words('english'))
```

When creating dictionaries of words it might be better to ignore stopwords in the given text. But this is a matter of programmer discretion. 

```python
newwords = [word for word in words if word not in englishstopwords]
print(newwords)
```

Print the count of items in the lists `words` and in `newwords`. Which words have been removed?

You can print the list of stopwords: `print(englishstopwords)`. Do you think more words could be added to the list?

#### Dictionaries in Python

Dictionaries are data structures that consist of elements, each having two components: a key and a value separated by a colon . The elements are enclosed in curly braces. Elements in a dictionary are separated by commas. An empty pair of curly braces can be used to create an empty dictionary.

`dict = {}`

A dictionary can also be created with initial values.

`dict = {'WI' : 'Wisconsin', 'TX' : 'Texas', 'AZ' : 'Arizona', 'CA' : 'California'}`

Dictionaries are unordered, changeable and indexed. Unlike lists and sets, dictionaries are indexed on the keys. The value for the key 'TX'  can be got by giving 'TX' as the index: `print(dict['TX'])`. 

Adding another element to the dictionary is straightforward: `dict['IL'] = 'Illinois'`. If the key already exists, then the value of that key will be changed, if not, a new element will be added to the dictionary.

<u>Some methods that operate on dictionaries:</u>

- Count of elements in the dictionary: `len(dict)`

- Remove the element with given key: `dict.pop['IL']`

- Clear the entire dictionary: `dict.clear()`

- Get a list of all the values in the dictionary: `dict.values()`

- Get a list o all the keys in the dictionary: `dict.keys()`

- Get a list of tuples of keys and values in the dictionary: `dict.items()`

Dictionaries are iterable (over keys) using for-loops. (*The following code prints the key and value for each element*):

```python
dict = {'WI' : 'Wisconsin', 'TX' : 'Texas', 'AZ' : 'Arizona', 'CA' : 'California'}
for state in dict:
    print(state, dict[state])
```

### Bible References and Concordance

The Bible is an interesting piece of literature since it is a collection of books neatly organized into chapters and verses. This exercise uses the King James Version text of the Bible which is available at: http://www.bibletech.net/BIBLE/Misc-tools-PlainText-KJV/kjvdat.txt

The first few lines of the file look like this. Notice how each verse is organized. Identify the components of each line (book name, chapter number, verse number, verse text). What is the delimiter used? How do each of the lines end?

```reStructuredText
Gen|1|1| In the beginning God created the heaven and the earth.~
Gen|1|2| And the earth was without form, and void; and darkness was upon the face of the deep. And the Spirit of God moved upon the face of the waters.~
```

Our first task is to create a concordance of words in the Bible. The dictionary will have the word as key and a list of verse references as values. Example: `{'crete' : ['Act 27:7', 'Act 27:12', 'Act 27:13', 'Act 27:21', 'Tit 1:5']}`.

First open the file. Then begin reading one line at a time from the file. Take each line and split it into its components. Create a "verse reference" by combining the book name, chapter number, and verse number. Take the verse text sans the last character. Create a list of the words in the verse, remove stopwords, and convert the words to lowercase. (All keys in our dictionary will be in lowercase). Update the dictionary with words as keys and verse references are values.

While adding verse references check to see whether a reference to the verse already exists for the given word. Duplicate verse references may get added if the given word occurs multiple times in the same verse.

```python
from nltk.corpus import stopwords
englishstopwords = set(stopwords.words('english'))
with open('kjvdat.txt', 'r') as infile:
    concordance = {}
    for line in infile:
        items = line.split('|')
        verse_ref = items[0]+" "+items[1]+":"+items[2]
        verse = items[3].strip()[0:-1]
        words = [word.strip(string.punctuation).lower() for word in verse.split() if word.strip(string.punctuation).lower() not in englishstopwords]
        for word in words:
            if word in concordance:
                if verse_ref not in concordance[word]:
                    concordance[word] += [verse_ref]
            else:
                concordance[word] = [verse_ref]
```

The concordance can be tested by providing a key: `print(concordance['joshua'])`. This print call will print the verse references for the given key ('joshua'). Try out other keys.

To get the actual text for each of the verse references of the given key we have to create a dictionary with the verse references as keys and the verse texts as values. In fact, this dictionary will be useful if a person wants to get the text of any given verse.

Add the following lines in the appropriate places in the above program.

`the_bible = {}`

`the_bible[verse_ref] = verse`

Now we can get the actual verses where the given word occurs (instead of just the references).

```python
searchkey = input("Which word are you searching for? ").lower()
if searchkey in concordance:
    for ref in concordance[searchkey]:
        print(ref, the_bible[ref])
else:
    print(searchkey,"not found in concordance.")
```

Or, the user can specify the book name, chapter and verse:

```python
versekey = input("Which verse do you want (Bbb cc:vv)?")
if versekey in the_bible:
    print(versekey, the_bible[versekey])
else:
    print(versekey,"not found in the_bible.")
```

The `versekey` should match the verse references in our dictionary.  Any additional characters entered may result in the reference not being found. 

Dictionary keys need not be single words (as in our concordance). It could contain phrases (bi-grams, trigrams, etc.). To be able to search for two-word phrases or three-word phrases we have to create the appropriate dictionaries. In order to be able to search for two-word or three-word phrases as they appear in The Bible, we have to retain the stop words as well.

Initialize two more dictionaries. Create bi-grams and tri-grams from the verse text. Add the bi-grams and tri-grams to the dictionary along with their verse references.

When creating bi-grams the `ngrams` function returns a list of tuples, each tuple having two elements like this: `[('in', 'the'), ('the', 'beginning'), ('beginning', 'God')...]`. However, it would be convenient if the keys in our dictionary were just one string (of two words) rather than a tuple. The two elements of the tuple can be joined using `" ".join()` and putting a space between the two words.

```python
from nltk.corpus import stopwords
from nltk import ngrams
englishstopwords = set(stopwords.words('english'))
with open('kjvdat.txt', 'r') as infile:
    concordance = {}
    the_bible = {}
    bg_concordance = {}
    tg_concordance = {}    
    for line in infile:
        items = line.split('|')
        verse_ref = items[0]+" "+items[1]+":"+items[2]
        verse = items[3].strip()[0:-1]
        the_bible[verse_ref] = verse
        words = [word.strip(string.punctuation).lower() for word in verse.split() if word.strip(string.punctuation).lower() not in englishstopwords]
        ngwords = [word.strip(string.punctuation).lower() for word in verse.split()]
        bigrams = [" ".join(bg) for bg in ngrams(ngwords, 2)]
        trigrams = [" ".join(bg) for bg in ngrams(ngwords, 3)]
        for word in words:
            if word in concordance:
                if verse_ref not in concordance[word]:
                    concordance[word] += [verse_ref]
            else:
                concordance[word] = [verse_ref]
        for bg in bigrams:
            if bg in bg_concordance:
                bg_concordance[bg] += [verse_ref]
            else:
                bg_concordance[bg] = [verse_ref]
        for tg in trigrams:
            if tg in tg_concordance:
                tg_concordance[tg] += [verse_ref]
            else:
                tg_concordance[tg] = [verse_ref]   
```

Try searching for two-word keys in the bg_concordance dictionary: `print(bg_concordance('jesus wept'))`. Try out other two-word keys. Search for three-word keys in the tg_concordance dictionary.

Write code to get search text from the user. Count the number of words in the input text. Search in the appropriate concordance and print out the verse references and text for the given search text. If the search text has only one word look for it in concordance. If it has two words look for it in bg_concordance. If it has three words look for it in tg_concordance. Does it make sense to create a dictionary with four-word keys? Create one if you think it will be useful.

```python
searchkey = input("Enter the text you are searching for: ").lower()
wordcount = len(searchkey.split())
if wordcount == 1:
    if searchkey in concordance:
        for verse_ref in concordance[searchkey]:
            print(verse_ref, the_bible[verse_ref])
    else:
        print(searchkey,": not found in concordance.")
elif wordcount == 2:
    if searchkey in bg_concordance:
        for verse_ref in bg_concordance[searchkey]:
            print(verse_ref, the_bible[verse_ref])
    else:
        print(searchkey,": not found in bg_concordance.")
elif wordcount == 3:
    if searchkey in tg_concordance:
        for verse_ref in tg_concordance[searchkey]:
            print(verse_ref, the_bible[verse_ref])
    else:
        print(searchkey,": not found in tg_concordance.")
else:
    print(len(searchkey.split()),"word dictionary does not exist.")
```

Since the text of The Bible does not change, the dictionaries created will not change either. Instead of having to re-create the dictionaries each time we need to search for text, we could save the created dictionaries and load them directly when we need to use them. That is where `pickle` comes in handy.

The `pickle` module converts a python object into a byte stream that can be written to disk. Unpickling is the inverse operation whereby a byte stream is converted back into an object. These processes are also referred to as serializing and de-serializing. Pickle files are given the filename extension `'.pkl'`. Each dictionary can be saved in a separate pickle file or all the dictionaries could be saved in the same file.  To save the object use `pickle.dump`.

```python
import pickle
with open('the_bible.pkl', 'wb') as handle:
    pickle.dump(the_bible, handle, protocol=pickle.HIGHEST_PROTOCOL)
```

To save all dictionaries in the same file create a tuple of all the dictionaries

```python
with open('alldicts.pkl', 'wb') as handle:
    pickle.dump((concordance,bg_concordance,tg_concordance,the_bible), handle, protocol=pickle.HIGHEST_PROTOCOL)
```

To de-serialize (unpickle) the file, use `pickle.load`:

```python
with open('the_bible.pkl', 'rb') as handle:
    the_bible = pickle.load(handle)
```

If all four dictionaries were saved in the same file, then load the pickle file into a tuple of four items.

```python
with open('alldicts.pkl', 'rb') as handle:
    conc, bg_conc, tg_conc, tbib = pickle.load(handle)

searchkey = input("Enter the text you are searching for: ").lower()
wordcount = len(searchkey.split())
if wordcount == 1:
    if searchkey in conc:
        for verse_ref in conc[searchkey]:
            print(verse_ref, tbib[verse_ref])
    else:
        print(searchkey,": not found in conc.")
elif wordcount == 2:
    if searchkey in bg_conc:
        for verse_ref in bg_conc[searchkey]:
            print(verse_ref, tbib[verse_ref])
    else:
        print(searchkey,": not found in bg_conc.")
elif wordcount == 3:
    if searchkey in tg_conc:
        for verse_ref in tg_conc[searchkey]:
            print(verse_ref, tbib[verse_ref])
    else:
        print(searchkey,": not found in tg_conc.")
else:
    print(len(searchkey.split()),"word dictionary does not exist.")
```

In the next part of this tutorial we will do additional things like identifying word collocations as well as searching for words with similar meanings. The searches should also be filterable by book, or section (Old Testament or New Testament). 

While creating dictionaries of bi-grams and tri-grams the keys were strings (of two words or three words). There may be times when retaining the tuples might be more efficient - especially when we want to do word stemming.

In addition, the library `tkinter` can be used to design a neat GUI for this search application.