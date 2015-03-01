# SenseRetrofit

Use this tool for retrofitting any word vector model to a sense ontology in order to derive sense specific vectors. The resulting models not only captures sense distinctions but are often empirically better on several semantic tasks. Details of the technique can be found in Jauhar et. al. (2015).

## Requirements

1. Python 2.7
	a. numpy
	b. scipy
	
## Data

#### Word Vectors
A file containing a pre-trained word vector model. The first line must specify the dimensions of the vector space model, after which one word vector per line is given. See ```data/samplevec.txt.gz```` for an example.

The output word vectors are in exactly the same format. For both, plain text and gzipped files are acceptable.

#### Sense Ontology
A file containing a local neighborhood description of a sense ontology. Again, both gzipped and plain text files are acceptable. Each line specifies one word sense and all it's neighbors with weights. The general format of a line is:

```<wordsense><weight> <neighbor-1><weight> <neighbor-2><weight> ...```

The first weight, corresponding to the word sense itself, is the sense agnostic weight, while all the others are the weights assigned to neighbors. See Jauhar et. al. (2015). for further details.

Two special characters must be chosen to denote a sense-separator and a value-separator. The former specifies a demarcator between a word's surface form and sense ID, while the latter does the same for a word sense unit and weight. Currently the two are set to ```%``` and ```#``` respectively. If you decide to use different values you must change the two corresponding global variables, ```senseSeparator``` and ```valueSeparator``` in the code of ```senseretrofit.py```.

For an example see ```data/sampleonto.txt.gz````, which contains a formatted version of the ontological graph of Wordnet 3.0, with the synonyms, hypernyms and hyponyms of all the word senses (except multi-word expressions).

## Running the code
```
$ python senseretrofit.py -v <vectorsFile> -q <ontologyFile> [-o outputFile] [-n numIters] [-e epsilon] [-h]
-v or --vectors to specify path to the word vectors input file (gzip or txt files are acceptable)
-q or --ontology to specify path to the ontology (gzip or txt files are acceptable)
-o or --output to optionally set path to output word sense vectors file (<vectorsFile>.sense is used by default)
-n or --numiters to optionally set the number of retrofitting iterations (10 is the default)
-e or --epsilon to optionally set the convergence threshold (0.001 is the default)
-h or --help (this message is displayed)
```

Try running the code with default settings on the sample vectors and ontology:

```python senseretrofit.py -v data/samplevec.txt.gz -q data/sampleonto.txt.gz```

## References

If you use this code please cite the following paper:

```
@InProceedings{jauhar:2015:NAACL,
  author    = {Jauhar, Sujay K.  and  Dyer, Chris and Hovy, Eduard},
  title     = {Ontologically Grounded Multi-sense Representation Learning for Semantic Vector Space Models},
  booktitle = {Proceedings of the 2015 Conference of the North American Chapter of the Association for Computational Linguistics: Human Language Technologies},
  year      = {2015},
}
```