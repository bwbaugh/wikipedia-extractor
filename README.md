This is a mirror repo for the script by Giuseppe Attardi, and contains history before the official repo started.

Please refer to the official repo if there any issues: https://github.com/attardi/wikiextractor

----

Wikipedia Extractor
===================

Introduction
------------

The project uses the *Italian Wikipedia* as source of documents for
several purposes: as training data and as source of data to be
annotated.

The Wikipedia maintainers provide, each month, an XML *dump* of all
documents in the database: it consists of a single XML file containing
the whole encyclopedia, that can be used for various kinds of analysis,
such as statistics, service lists, etc.

Wikipedia dumps are available from [Wikipedia database
download](http://dumps.wikimedia.org/).

The Wikipedia extractor tool generates plain text from a Wikipedia
database dump, discarding any other information or annotation present in
Wikipedia pages, such as images, tables, references and lists.

Each document in the dump of the encyclopedia is representend as a
single XML element, encoded as illustrated in the following example from
the document titled *Armonium*:

     <page>
     <title>Armonium</title>
     <id>2</id>
     <timestamp>2008-06-22T21:48:55Z</timestamp>
     <username>Nemo bis</username>
     <comment>italiano</comment>
     <text xml:space="preserve">thumb|right|300 px

     L'armonium' (in francese, harmonium) è uno
      strumento musicale azionato con una tastiera, detta
     manuale. Sono stati costruiti anche alcuni armonium con due manuali.

     ==Armonium occidentale==
     Come l'organo, l'armonium è utilizzato tipicamente in
     chiesa, per l'esecuzione di musica sacra, ed è
     fornito di pochi registri, quando addirittura in certi casi non ne possiede
     nemmeno uno: il suo timbro è molto meno ricco di quello
     organistico e così pure la sua estensione.

     ...

     ==Armonium indiano==
     Template:S sezione

     == Voci correlate ==
     *Musica
     *Generi musicali</text>

For this document the Wikipedia extractor produces the following plain
text:

    <doc id="2" url="http://it.wikipedia.org/wiki/Armonium">
    Armonium.
    L'armonium (in francese, “harmonium”) è uno strumento musicale azionato con
    una tastiera, detta manuale. Sono stati costruiti anche alcuni armonium con
    due manuali.

    Armonium occidentale.
    Come l'organo, l'armonium è utilizzato tipicamente in chiesa, per l'esecuzione
    di musica sacra, ed è fornito di pochi registri, quando addirittura in certi
    casi non ne possiede nemmeno uno: il suo timbro è molto meno ricco di quello
    organistico e così pure la sua estensione.
    ...
    </doc>

The extraction tool is written in Python and requires no additional
library. it aims to achieve high accuracy in extraction task.

Wikipedia articles are written in the [MediaWiki Markup
Language](http://www.mediawiki.org/wiki/Help:Formatting), which provides
a simple notation for formatting text (bolds, italics, underlines,
images, tables, etc.). It is also posible to insert HTML markup in the
documents. Wiki and HTML tags are often misused (unclosed tags, wrong
attributes, etc.), therefore the extractor deploys several heuristics in
order to circumvent such problems. A currently missing feature for the
extractor is template expansion.

Description
-----------

[WikiExtractor.py](http://medialab.di.unipi.it/wiki/Wikipedia_Extractor)
is a Python script that extracts and cleans text from a [Wikipedia
database dump](http://dumps.wikimedia.org/). The output is stored in a
number of files of similar size in a given directory. Each file contains
several documents in the [document
format](/wiki/Document_Format "Document Format").

Usage:

     WikiExtractor.py [options]

Options:

     -c, --compress        : compress output files using bzip
     -b, --bytes= n[KM]    : put specified bytes per output file (default 500K)
     -B, --base= URL       : base URL for the Wikipedia pages
     -o, --output= dir     : place output files in specified directory (default
                             current)
     -l, --link            : preserve links
     --help                : display this help and exit

Example of Use
--------------

The following commands illustrate how to apply the script to a Wikipedia
dump:

    > wget http://download.wikimedia.org/itwiki/latest/itwiki-latest-pages-articles.xml.bz2
    > bzcat itwiki-latest-pages-articles.xml.bz2 |
      WikiExtractor.py -cb 250K -o extracted

In order to combine the whole extracted text into a single file one can
issue:

    > find extracted -name '*bz2' -exec bunzip2 -c {} \; > text.xml
    > rm -rf extracted

Related Work
------------

-   [WikiPrep](http://www.cs.technion.ac.il/~gabr/resources/code/wikiprep/)
    A Perl tool for preprocessing Wikipedia XML dumps.
-   [Extracting Text from
    Wikipedia](http://evanjones.ca/software/wikipedia2text.html) Another
    Python tool for text extracting from Wikipedia XML dumps.
-   [Alternative
    Parsers](http://www.mediawiki.org/wiki/Alternative_parsers) A list
    of links, descriptions, and status reports of the various
    alternative MediaWiki parsers.

