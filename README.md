
EHU-Coreference_EN_kernel
=========================

The CorefGraph-en module provides an implementation of the Multi-Sieve Pass system for
for Coreference Resolution system originally proposed by the Stanford NLP
Group (Raghunathan et al., 2010; Lee et al., 2011) and (Lee et al., 2013).
This system proposes a number of deterministic passes, ranging from high precision to
higher recall, each dealing with a different manner in which coreference
manifests itself in running text.

Although more sieves are available, in order to facilitate the integration of
the coreference system for the 6 languages of OpeNER we have included here 4 sieves:
Exact String Matching, Precise Constructs, Strict Head Match and Pronoun Match (the
sieve nomenclature follows Lee et al (2013)). Furthermore, as it has been reported,
this sieves are responsible for most of the performance in the Stanford system.

The implementation is a result of a collaboration between the IXA NLP (http://ixa.si.ehu.es) and
LinguaMedia Groups (http://linguamedia.deusto.es).

Contents
========

The contents of the CorefGraph module are the following:

    + features/	    Gender and Number feature extraction of pronouns
    + graph/	    Graph utils for traversal of Syntactic Trees. Used for Antecedent Selection mainly.
    + multisieve/   Implemented Sieve passes plus several dictionaries.
    + output/	    Various output utilities
    + process.py    Main module. Use this to execute the system.
    + pykaf/	    KAF output utilities
    + resources/    Most dictionaries (gender, number, animacy, demonyms) are placed here.
    + test/	    Testing funcionalities

- README.md: This README


INSTALLATION
============

In order to run CorefGraph coreference module you need to install the python graph library
graph-tool.

http://projects.skewed.de/graph-tool/

If you run a Debian based linux system such as Ubuntu or Linux Mint, you can easily install
graph-tool and all its dependencies by apt-get the packages built by the graph-tool developer.

If you run a Red Hat based server you can try to use alien to generate .rpm packages from the .deb
ones or you can try and compile from source (good luck).

USING CorefGraph-en
===================

CorefGraph-en requires two inputs (you can see examples in the resources/examples directory):

1. KAF with wf, terms and entities elements.
2. Constituent syntactic analysis in Treebank format, one sentence per line with heads marked.

To run the program execute:

````shell
python process.py -i input.kaf input.treebank
````

CorefGraph-en will output KAF via standard output with the <coreference> clusters added to the KAF input received. Note
that for the full functionality of CorefGraph you will need to provide the treebank input with the heads of (at least) the
Noun Phrases marked, as it can be seen in the treebank input examples in the resource/examples directory. If you do not provide
heads, only Exact String Match will work properly, whereas Precise Constructs, Strict Head Match and Pronoun Match will not.

For a full explanation of how the Multi Sieve Pass system works see documentation in resources/doc.

ADAPTING CorefGraph-en to your language
=======================================

There are a number of changes needed to be made to make CorefGraph works for other languages. Although we have try to
keep the language dependent features to a minimum, you will still need to create some dictionaries for your own language
and make some very minor changes in the code. Here is the list of very file in the Corefgraph module that needs to be changed:

+ features/ grendel.py check POS tags
+ graph/ kaf.py check head =h mentions
+ multisieve/
    - dictionaries.py
    - mentionSelection.py, POS and Syntactic tags
+ resources/
    - animacy.py NE tags
    - demonym
        - en,es wiki and txt files
        - genti.py change parameters of iles and regexp to process
          wikipedia page
        - files/ Lin et al gender, number and animacy dictionaries (see references in Multi Sieve papers)
        - gender.py parameters (although this should not be necessary
          if files names in files/ directory do not change)
        - number.py as above
        - pronouns.py change lists of pronouns depending on language


Contact Information
===================

````shell
Rodrigo Agerri
IXA NLP Group
University of the Basque Country (UPV/EHU)
E-20018 Donostia-San Sebastian
rodrigo.agerri@ehu.es
````
