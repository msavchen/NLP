# NLP
Natural language processing from AGH university

# 1. Regular expressions (aka regexps)

The task is concentrated on using regular expressions for extracting basic information from textual data. 
You will get more familiar with the regexp features that are particularly important in natural language processing.

## Task

A dataset containing texts of Polish statutory law is available at [http://apohllo.pl/text/ustawy.tar.gz](http://apohllo.pl/text/ustawy.tar.gz).

It contains texts of Polish bills, e.g.:

```
Tekst ustawy przyjęty przez Senat bez poprawek
 
USTAWA
z
dnia 8 listopada 2013 r.
 
o
zmianie niektórych ustaw w związku z realizacją ustawy budżetowej[1])
 
Art.
1. 
W
ustawie z dnia 4 marca 1994 r. o zakładowym funduszu świadczeń socjalnych (Dz. U.
z 2012 r. poz. 592, z późn. zm.[2]))
po art. 5b dodaje się art. 5c w brzmieniu:
„Art. 5c. W 2014 r. przez
przeciętne wynagrodzenie miesięczne w gospodarce narodowej, o którym mowa w art.
5 ust. 2, należy rozumieć przeciętne wynagrodzenie miesięczne w gospodarce narodowej
w drugim półroczu 2010 r. ogłoszone przez Prezesa Głównego Urzędu Statystycznego
na podstawie art. 5 ust. 7.”.
```

Task objectives:

1. For each bill compute the number of the following amendments present in the bill:
   * addition of a unit (e.g. **dodaje się ust. 5a**),
   * removal of a unit (e.g. **w art. 10 ust. 1 pkt 8 skreśla się**),
   * change of a unit (e.g. **art. 5 otrzymuje brzmienie**).
1. Note that other types of changes, e.g. **po wyrazach "na dofinansowanie" dodaje się wyrazy " , z zastrzeżeniem art. 21a,"**, must not be included in the result.
1. Plot results from point 1 showing how the percentage of amendments of a given type changed in the consecutive years.
1. Compute the total number of occurrences of the word **ustawa** in any inflectional form (*ustawa*, *ustawie*, *ustawę*, etc.)
   and all spelling forms (*ustawa*, *Ustawa*, *USTAWA*), excluding other words with the same prefix (e.g. *ustawić*).
1. Compute the total number of occurrences of the same word (same conditions), followed by **z dnia** expression.
1. As above, but **not** followed by **z dnia** expression. Is the result correct (result 4 =? result 5 + result 6)?
1. Compute the total number of occurrences of the word **ustawa** in any inflectional form, excluding occurrences
   following **o zmianie** expression.
1. Plot results 4-7 using a bar chart.

# 2. Lemmatization and full text search (FTS)

The task is concentrated on using full text search engine (ElasticSearch) to perform basic search
operations in a text corpus.

## Tasks

1. Install ElasticSearch (ES).
1. Install an ES plugin for Polish https://github.com/allegro/elasticsearch-analysis-morfologik 
1. Define an ES analyzer for Polish texts containing:
   1. standard tokenizer
   1. synonym filter with the following definitions:
      1. kpk - kodeks postępowania karnego
      1. kpc - kodeks postępowania cywilnego
      1. kk - kodeks karny
      1. kc - kodeks cywilny
   1. Morfologik-based lemmatizer
   1. lowercase filter
1. Define an ES index for storing the contents of the legislative acts.
1. Load the data to the ES index.
1. Determine the number of legislative acts containing the word **ustawa** (in any form).
1. Determine the number of legislative acts containing the words **kodeks postępowania cywilnego** 
   in the specified order, but in any inflection form.
1. Determine the number of legislative acts containing the words **wchodzi w życie** 
   (in any form) allowing for up to 2 additional words in the searched phrase.
1. Determine the 10 documents that are the most relevant for the phrase **konstytucja**.
1. Print the excerpts containing the word **konstytucja** (up to three excerpts per document) 
   from the previous task.


# 3. Levenshtein distance and spelling corrections

The task introduces the Levenshtein distance - a measure that is useful in tasks such as approximate string matching.

## Tasks

1. Use SpaCy [tokenizer API](https://spacy.io/api/tokenizer) to tokenize the text from the law corpus.
1. Compute **frequency list** for each of the processed files.
1. Aggregate the result to obtain one global frequency list.
1. Reject all entries that are shorter than 2 characters or contain non-letter characters (make sure to include Polish
   diacritics).
1. Make a plot in a logarithmic scale:
   1. X-axis should contain the **rank** of a term, meaning the first rank belongs to the term with the highest number of
      occurrences; the terms with the same number of occurrences should be ordered by their name,
   2. Y-axis should contain the **number of occurrences** of the term with given rank.
1. Download [polimorfologik.zip](https://github.com/morfologik/polimorfologik/releases/download/2.1/polimorfologik-2.1.zip) dictionary
   and use it to find all words that do not appear in that dictionary.
1. Find 30 words with the highest ranks that do not belong to the dictionary.
1. Find 30 words with 5 occurrences that do not belong to the dictionary.
1. Use Levenshtein distance and the frequency list, to determine the most probable correction of the words from the
   second list. (You don't have to apply the distance directly. Any method that is more efficient than scanning the
   dictionary will be appreciated.)
1. Load Morfologik to ElasticSearch (one document for each form) and use fuzzy matching to obtain the possible
   corrections of the 30 words with 5 occurrences that do not belong to the dictionary.
1. Compare the results of your algorithm and output of ES. 
1. Draw conclusions regarding:
   * the distribution of words in the corpus,
   * the number of true misspellings vs. the number of unknown words,
   * the performance of your method compared to ElasitcSearch,
   * the results provided by your method compared to ElasticSearch,
   * the validity of the obtained corrections.
