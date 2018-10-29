## Dictionaries in Python

### Task List:

#### Preliminaries:

- Download the text file.
- The Bible is a collection of books each organized into chapters and verses.
- The books in The Bible are grouped under two sections: Old Testament and New Testament.
- Open the file in a text editor and view the file contents. See how each line is organized.
- Ask the user to input a word (or phrase) and print all lines in the file that contain that word (or phrase).
- Ask the user to input a word (or phrase) and print the verse reference that contains that word (or phrase). Verse reference will include the book, chapter and verse in the format (Bbb cc::vv).
- Speeding things up using dictionaries.
  - "Tn the beginning when God began to create......."
  - {'in' : ['Gen 1:1'], 'the' : ['Gen 1:1'], 'beginning' : ['Gen 1:1'], 'when' : ['Gen 1:1'], 'god' : ['Gen 1:1'], ...}
  - To search for a word, just look it up in the dictionary and print the references. No need to iterate through entire file each time.

#### Using NLTK:

- Download NLTK (pip import nltk)
- Download NLTK Corpora (import nltk; nltk.download())
- Tokenization (sent_tokenize, word_tokenize, WordPunctTokenizer)
- N-gramming (ngrams, bigrams, trigrams)
- Stopwords

#### Creating Dictionaries:

- Create a concordance of words in The Bible.
- Test it by asking user input and printing the verse references for that word.
- Create a dictionary of verse references (keys) and verse text (values).
- Print out verse text instead of verse references for input word.
- Create a dictionary of bi-grams of verse texts.
- Test by asking user input and printing verse text  for that input.
- Create a dictionary of tri-grams of verse texts.
- Test the tri-gram dictionary.
- Ask user input. Depending on the number of words in the input, search the concordance, or bi-gram dictionary, or tri-gram dictionary for matches and print the relevant verse texts.

#### Pickle

- install pickle (pip install pickle)

- Saving the work (or sharing the work).
- Pickle the dictionary for later use. (pickle.dump)
- Unpickling the dictionary (pickle.load)