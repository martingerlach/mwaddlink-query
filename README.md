# mwaddlink-query
querying a trained model for link recommendation with the MediaWiki AddLink Extension Model and API

## Introduction
This repository tries to provide an interface to the link-recommendation models trained with [mwaddlink](https://github.com/dedcode/mwaddlink).

Update 2020-10-15:
- The content of this repo has been merged intothe main repo [mwaddlink](https://github.com/dedcode/mwaddlink)
- So check out the code there, this repo will not be maintained anymore and is most likely out of date.

Currently, the model (and the corresponding data) lives on the stat1008-machine on the wmf-cluster. So this will only work there for the moment.

Note that we only support a small number of languages so far (simple, de), but are continuouosly expanding. 

## Setup

Clone the repo
```bash
git clone https://github.com/martingerlach/mwaddlink-query
cd mwaddlink-query
```

Set up the virtual environment
```bash
virtualenv -p /usr/bin/python3 venv/
source venv/bin/activate
pip install -r requirements.txt
```

Additional notes:
- Make sure you have the http-proxy set up https://wikitech.wikimedia.org/wiki/HTTP_proxy
- Add the following nltk-package: ```python -m nltk.downloader punkt```

## Running the model

Get recommendations for a page in a wiki

```bash
python addlink-query_links.py -l <LANG> -p <PAGE>
```

for example,
```bash
python addlink-query_links.py -l de -p Garnet_Carter
```
this will yield a json with the following info:
- 'page_title' (from input)
- 'lang' (from input)
- 'pageid', the page-id of the page in the respective wiki
- 'revid', the revision-id of the wikitext of the page we are working with
- 'no_added_links', the number of recommended links
- 'added_links', a list with a dictionary for each recommended link
    - 'linkTarget': the pagetitle of the recommended link,
    - 'anchor':the anchor-text of the recommendaed link,
    - 'probability':the probability of the model for the link (confidence)
    - 'startOffset':character-offset start of anchor in wikitext (get via revid)
    - 'endOffset':character-offset end of anchor in wikitext (get via revid)

Other optional arguments_
- -t, threshold for link recommendation (float, default=0.9)
- -o, output-path for output-json (str, default="" will print to terminal)