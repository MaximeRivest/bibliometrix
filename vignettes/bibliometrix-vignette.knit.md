---
title: "A brief introduction to bibliometrix"
author: "Massimo Aria and Corrado Cuccurullo"
date: "2017-08-22"
output: rmarkdown::html_vignette
vignette: >
  %\VignetteIndexEntry{A brief introduction to bibliometrix}
  %\VignetteEngine{knitr::rmarkdown}
  %\VignetteEncoding{UTF-8}
---

#### Latest version 1.6 
#### http://www.bibliometrix.org

#### Citation for package 'bibliometrix': 


```r
citation("bibliometrix")
```

```
## 
## To cite bibliometrix in publications, please use:
## 
##   Aria, Massimo and Cuccurullo, Corrado (2016). bibliometrix: a R
##   tool for comprehensive bibliometric analysis of scientific
##   literature. http://www.bibliometrix.org
## 
## A BibTeX entry for LaTeX users is
## 
##   @Manual{,
##     title = {bibliometrix: an R tool for comprehensive bibliometric analysis of scientific literature},
##     author = {{Aria} and {Massimo} and {Cuccurullo} and {Corrado}},
##     organization = {University of Naples Federico II},
##     address = {Naples, Italy},
##     year = {2016},
##   }
```


## Introduction

**bibliometrix** package provides a set of tools for quantitative research in bibliometrics and scientometrics.

Bibliometrics turns the main tool of science, quantitative analysis, on itself. Essentially, bibliometrics is the application of quantitative analysis and statistics to publications such as journal articles and their accompanying citation counts. Quantitative evaluation of publication and citation data is now used in almost all science fields to evaluate growth, maturity, leading authors, conceptual and intellectual maps, trends of a scientific community.

Bibliometrics is also used in research performance evaluation, especially in university and government labs, and also by policymakers, research directors and administrators, information specialists and librarians, and scholars themselves.

**bibliometrix** supports scholars in three key phases of analysis:

* Data importing and conversion to R format; 

* Bibliometric analysis of a publication dataset;

* Building matrices for co-citation, coupling, collaboration, and co-word analysis. Matrices are the input data for performing network analysis, multiple correspondence analysis, and any other data reduction technique.

## Bibliographic databases
**bibliometrix** works with data extracted from the two main bibliographic databases: SCOPUS and Thomson Reuter ISI Web of Knowledge.

*SCOPUS* (http://www.scopus.com), founded in 2004, offers a great deal of flexibility for the bibliometric user.  It permits to query for different fields, such as titles, abstracts, keywords, references and so on. SCOPUS allows for relatively easy downloading data-queries, although there are some limits on very large results sets with over 2,000 items.

*ISI Web of Knowledge (WoK)* (http://www.webofknowledge.com), owned by Thomson Reuter, was founded by Eugene Garfield, one of the pioneers of bibliometrics.  
This platform includes many different collections.

## Data acquisition
Bibliographic data may be obtained by querying the SCOPUS or ISI WoK database by diverse fields, such as topic, author, journal, timespan, and so on.

In this example, we show how to download data, querying a term in the manuscript title field.

We choose the generic term "bibliometrics".

### Querying from ISI WoK
At the link http://www.webofknowledge.com , select Web of Science Core Collection database. 

Write the keyword "bibliometrics" in the search field and select title from the dropdown menu (see figure 1).  

<div style="width:300px; height=200px">
![Figure 1](figures/isi1.png)
</div>


Choose SCI-EXPANDED and SSCI citation indexes.

The search yielded 291 results on May 09, 2016.

Results can be refined using options on the left side of the page (type of manuscript, source, subject category, etc.).

After refining the query, you can add records to your Marked List by clicking the button "add to marked list" at the end of the page and selecting the records to save (see figure 2).

<div style="width:300px; height=200px">
![Figure 2](figures/isi2.png)
</div>


The Marked List page provides you with a list of publications selected and various means of exporting data. 

To export the data you desire, choose the export tool and follow the three intuitive steps (see figure 3).

<div style="width:300px; height=200px">
![Figure 3](figures/isi3.png)
</div>


The export tool allows you to select the diverse fields to save. So, select the fields your are interested in (for example all the available data about marked records).


To download an export file, in an appropriate format for *bibliometrix* package, make sure to select the option "**Save to Other File Formats**" and choose Bibtex or Plain Text.

Please pay attention that bibtex import function is faster than plain text.

The ISI platform permits to export only 500 records at a time. 

The ISI Web of Science export tool creates an export file with a default name "savedrecs" with extention ".txt" or ".bib" for plain text or bibtex format respectively. Export files can be separately stored.

### Querying from SCOPUS
The access to SCOPUS is via http://www.scopus.com.

To find all articles whose title includes the term "bibliometrics", simply write this keyword in the field and select "Article Title" (see figure 4)

<div style="width:300px; height=200px">
![Figure 4](figures/scopus1.png)
</div>

The search yielded 414 results on May 09, 2016.

You can download the references (up to 2,000 full records) by checking the 'Select All' box and clicking on the link 'Export'. 
Choose the file type "bibtex export" and "all available information" (see figure 5).


<div style="width:300px; height=200px">
![Figure 5](figures/scopus2.png)
</div>


The SCOPUS export tool creates an export file with the default name "scopus.bib".

## bibliometrix installation

Download and install the most recent version of R (https://cran.r-project.org/)

Download and install the most recent version of Rstudio (http://www.rstudio.com)

Open Rstudio, in the console window, digit:

install.packages("bibliometrix", dependencies=TRUE)      ### installs bibliometrix package and dependencies



```r
library(bibliometrix)   ### load bibliometrix package
```

```
## 
## bibliometrix
## A R tool for comprehensive bibliometric analysis of scientific literature
## 
## by Massimo Aria & Corrado Cuccurullo
## 
## http:\\www.bibliometrix.org
```

## Data loading and converting

The export file can be read by R using the function *readFiles*:


```r
D <- readFiles("http://www.bibliometrix.org/datasets/savedrecs.bib")
```

D is a large character vector. 
*readFiles* argument contains the name of files downloaded from SCOPUS or ISI WOS website.

The function *readFiles* combines all the text files onto a single large character vector. Furthermore, the format is converted into UTF-8.

es. D <- readFiles("file1.txt","file2.txt", ...)



The object D can be converted in a  data frame using the function *convert2df*:

```r
M <- convert2df(D, dbsource = "isi", format = "bibtex")
```

```
## Articles extracted   100 
## Articles extracted   200 
## Articles extracted   291
```

*convert2df* creates a bibliographic data frame with cases corresponding to manuscripts and variables to Field Tag in the original export file.

Each manuscript contains several elements, such as authors' names, title, keywords and other information. All these elements constitute the bibliographic attributes of a document, also called metadata.

Data frame columns are named using the standard ISI WoS Field Tag codify. 

The main field tags are:

Field Tag  | Description
---------- | -----------
AU		     | Authors
TI		     | Document Title
SO		     | Publication Name (or Source)
JI		     | ISO Source Abbreviation
DT		     | Document Type
DE		     | Authors' Keywords
ID		     | Keywords associated by SCOPUS or ISI database
AB		     | Abstract
C1		     | Author Address
RP		     | Reprint Address
CR		     | Cited References
TC		     | Times Cited
PY		     | Year
SC		     | Subject Category
UT		     | Unique Article Identifier
DB		     | Bibliographic Database


For a complete list of field tags see https://images.webofknowledge.com/WOK50B6/help/WOS/h_fieldtags.html

## Bibliometric Analysis

The first step is to perform a descriptive analysis of the bibliographic data frame.

The function *biblioAnalysis* calculates main bibliometric measures using this syntax:
 

```r
results <- biblioAnalysis(M, sep = ";")
```

The function *biblioAnalysis* returns an object of class "bibliometrix".

An object of class "bibliometrix" is a list containing the following components:

List element       | Description
------------------ | --------------------------------------------
Articles		 | the total number of manuscripts
Authors		   | the authors' frequency distribution
AuthorsFrac	 | the authors' frequency distribution (fractionalized)
FirstAuthors | first author of each manuscript
nAUperPaper	 | the number of authors per manuscript
Apparences	 |  the number of author apparences
nAuthors		 | the number of authors
AuMultiAuthoredArt | the number of authors of multi authored articles
MostCitedPapers | the list of manuscripts sorted by citations
Years		     | pubblication year of each manuscript
FirstAffiliation | the affiliation of the first author
Affiliations | the frequency distribution of affiliations (of all co-authors for each paper)
Aff_frac		 | the fractionalized frequency distribution of affiliations (of all co-authors for each paper)
CO		       | the affiliation country of first author
Countries		 | the affiliation countries' frequency distribution
TotalCitation | 		 the number of times each manuscript has been cited
TCperYear		 | the yearly average number of times each manuscript has been cited
Sources		   | the frequency distribution of sources (journals, books, etc.)
DE		       | the frequency distribution of authors' keywords
ID		       | the frequency distribution of keywords associated to the manuscript by SCOPUS and Thomson Reuters' ISI Web of Knowledge databases

### Functions *summary* and *plot*

To summarize main results of the bibliometric analysis, use the generic function *summary*.
It displays main information about the bibliographic data frame and several tables, such as annual scientific production, top manuscripts per number of citations, most productive authors, most productive countries, total citation per country, most relevant sources (journals) and most relevant keywords.

*summary* accepts two additional arguments. *k* is a formatting value that indicates the number of rows of each table. *pause* is a logical value (TRUE or FALSE) used to allow (or not) pause in screen scrolling.
Choosing k=10 you decide to see the first 10 Authors, the first 10 sources, etc.


```r
S=summary(object = results, k = 10, pause = FALSE)
```

```
## 
## 
## Main Information about data
## 
##  Articles                              291 
##  Sources (Journals, Books, etc.)       141 
##  Keywords Plus (ID)                    474 
##  Author's Keywords (DE)                365 
##  Period                                1985 - 2015 
##  Average citations per article         11.73 
## 
##  Authors                               546 
##  Author Appearances                    648 
##  Authors of single authored articles   109 
##  Authors of multi authored articles    437 
## 
##  Articles per Author                   0.533 
##  Authors per Article                   1.88 
##  Co-Authors per Articles               2.23 
##  Collaboration Index                   2.97 
##  
## 
## Annual Scientific Production
## 
##  Year    Articles
##     1985        4
##     1986        3
##     1987        6
##     1988        7
##     1989        8
##     1990        6
##     1991        7
##     1992        6
##     1993        5
##     1994        7
##     1995        1
##     1996        8
##     1997        4
##     1998        5
##     1999        2
##     2000        7
##     2001        8
##     2002        5
##     2003        1
##     2004        3
##     2005       12
##     2006        5
##     2007        5
##     2008        8
##     2009       14
##     2010       17
##     2011       20
##     2012       25
##     2013       21
##     2014       29
##     2015       32
## 
## Annual Percentage Growth Rate 7.177346 
## 
## 
## Most Productive Authors
## 
##            Authors        Articles Authors        Articles Fractionalized
## 1  BORNMANN,LUTZ                 8 BORNMANN,LUTZ                     4.67
## 2  KOSTOFF,RN                    8 MARX,WERNER                       3.17
## 3  MARX,WERNER                   6 ATKINSON,ROGER                    3.00
## 4  HUMENIK,JA                    5 BROADUS,RN                        3.00
## 5  ABRAMO,GIOVANNI               4 CRONIN,B                          3.00
## 6  D'ANGELO,CIRIACOANDREA        4 BORGMAN,CL                        2.50
## 7  GLANZEL,W                     4 MCCAIN,KW                         2.50
## 8  ATKINSON,ROGER                3 PERITZ,BC                         2.50
## 9  BARKER,K                      3 KOSTOFF,RN                        2.10
## 10 BORGMAN,CL                    3 ADAMS,JONATHAN                    2.00
## 
## 
## Top manuscripts per citations
## 
##                                                                                             Paper         
## 1  DAIM TUGRULU;RUEDA GUILLENNO;MARTIN HILARY;GERDSRI PISEK,(2006),TECHNOL. FORECAST. SOC. CHANG.         
## 2  WHITE HD;MCCAIN KW,(1989),ANNU. REV. INFORM. SCI. TECHNOL.                                             
## 3  BORGMAN CL;FURNER J,(2002),ANNU. REV. INFORM. SCI. TECHNOL.                                            
## 4  WEINGART P,(2005),SCIENTOMETRICS                                                                       
## 5  NARIN F,(1994),SCIENTOMETRICS                                                                          
## 6  CRONIN B,(2001),J. INF. SCI.                                                                           
## 7  CHEN YU-CHUN;YEH HSIAO-YUN;WU JAU-CHING;HASCHLER INGO;CHEN TZENG-JI;WETTER THOMAS,(2011),SCIENTOMETRICS
## 8  HOOD WW;WILSON CS,(2001),SCIENTOMETRICS                                                                
## 9  D'ANGELO CIRIACOANDREA;GIUFFRIDA CRISTIANO;ABRAMO GIOVANNI,(2011),J. AM. SOC. INF. SCI. TECHNOL.       
## 10 NARIN F;OLIVASTRO D;STEVENS KA,(1994),EVAL. REV.                                                       
##     TC TCperYear
## 1  211     19.18
## 2  196      7.00
## 3  192     12.80
## 4  151     12.58
## 5  141      6.13
## 6  129      8.06
## 7  101     16.83
## 8   71      4.44
## 9   64     10.67
## 10  62      2.70
## 
## 
## Most Productive Countries
## 
##    Country   Articles   Freq
## 1  USA             81 0.3034
## 2  ENGLAND         26 0.0974
## 3  GERMANY         17 0.0637
## 4  FRANCE          13 0.0487
## 5  BRAZIL          12 0.0449
## 6  CHINA           10 0.0375
## 7  INDIA           10 0.0375
## 8  SPAIN            9 0.0337
## 9  AUSTRALIA        8 0.0300
## 10 CANADA           8 0.0300
## 
## 
## Total Citations per Country
## 
##    Country      Total Citations Average Article Citations
## 1  USA                     1831                     22.60
## 2  GERMANY                  330                     19.41
## 3  ITALY                    163                     32.60
## 4  AUSTRALIA                134                     16.75
## 5  ENGLAND                  121                      4.65
## 6  CANADA                   111                     13.88
## 7  INDIA                     85                      8.50
## 8  SPAIN                     85                      9.44
## 9  IRAN                      74                     37.00
## 10 BELGIUM                   70                     10.00
## 
## 
## Most Relevant Sources
## 
##                                                            Sources       
## 1  SCIENTOMETRICS                                                        
## 2  JOURNAL OF THE AMERICAN SOCIETY FOR INFORMATION SCIENCE AND TECHNOLOGY
## 3  JOURNAL OF THE AMERICAN SOCIETY FOR INFORMATION SCIENCE               
## 4  JOURNAL OF DOCUMENTATION                                              
## 5  JOURNAL OF INFORMATION SCIENCE                                        
## 6  JOURNAL OF INFORMETRICS                                               
## 7  BRITISH JOURNAL OF ANAESTHESIA                                        
## 8  LIBRI                                                                 
## 9  SOCIAL WORK IN HEALTH CARE                                            
## 10 TECHNOLOGICAL FORECASTING AND SOCIAL CHANGE                           
##    Articles
## 1        49
## 2        14
## 3         8
## 4         6
## 5         6
## 6         6
## 7         5
## 8         5
## 9         5
## 10        5
## 
## 
## Most Relevant Keywords
## 
##    Author Keywords (DE)      Articles Keywords-Plus (ID)     Articles
## 1      BIBLIOMETRICS               63    SCIENCE                   38
## 2      CITATION ANALYSIS           11    INDICATORS                24
## 3      SCIENTOMETRICS               7    IMPACT                    23
## 4      IMPACT FACTOR                5    CITATION                  20
## 5      INFORMATION RETRIEVAL        5    CITATION ANALYSIS         15
## 6      PEER REVIEW                  5    JOURNALS                  14
## 7      CITATION                     4    H-INDEX                   13
## 8      CITATIONS                    4    PUBLICATION               12
## 9      H-INDEX                      4    INFORMATION-SCIENCE       10
## 10     IMPACT FACTORS               4    IMPACT FACTORS             8
```

Some basic plots can be drawn using the generic function \code{plot}:


```r
plot(x = results, k = 10, pause = FALSE)
```

![](bibliometrix-vignette_files/figure-html/plot generic function-1.png)<!-- -->![](bibliometrix-vignette_files/figure-html/plot generic function-2.png)<!-- -->![](bibliometrix-vignette_files/figure-html/plot generic function-3.png)<!-- -->![](bibliometrix-vignette_files/figure-html/plot generic function-4.png)<!-- -->

## Analysis of Cited References 
The function *citations* generates the frequency table of the most cited references or the most cited first authors (of references).

For each manuscript, cited references are in a single string stored in the column "CR" of the data frame. 

For a correct extraction, you need to identify the separator field among different references, used by ISI or SCOPUS database. Usually, the default separator is ";" or `".  "` (a dot with double space).


```r
# M$CR[1]
```
The figure shows the reference string of the first manuscript. In this case, the separator field is `sep = ".  "`.

<div style="width:300px; height=200px">
![Figure 6](figures/cr1.png)
</div>


To obtain the most frequent cited manuscripts:

```r
CR <- citations(M, field = "article", sep = ".  ")
CR$Cited[1:10]
```

```
## CR
## HIRSCH JE, 2005, P NATL ACAD SCI USA, V102, P16569, DOI 101073/PNAS0507655102 
##                                                                            26 
##       SMALL H, 1973, J AM SOC INFORM SCI, V24, P265, DOI 101002/ASI4630240406 
##                                                                            19 
##                                   DE SOLLA PRICE DJ, 1963, LITTLE SCI BIG SCI 
##                                                                            15 
##                                             PRITCHARA, 1969, J DOC, V25, P348 
##                                                                            14 
##                             BRADFORD S C, 1934, ENGINEERING-LONDON, V137, P85 
##                                                                            13 
##       GARFIELD E, 2006, JAMA-J AM MED ASSOC, V295, P90, DOI 101001/JAMA295190 
##                                                                            11 
##                                    COLE FRANCIS J, 1917, SCI PROGR, V11, P578 
##                                                                            10 
##                  KESSLER MM, 1963, AM DOC, V14, P10, DOI 101002/ASI5090140103 
##                                                                            10 
##         SMALL HG, 1978, SOC STUD SCI, V8, P327, DOI 101177/030631277800800305 
##                                                                            10 
##                                         SMITH LC, 1981, LIBR TRENDS, V30, P83 
##                                                                            10
```

To obtain the most frequent cited first authors:

```r
CR <- citations(M, field = "author", sep = ".  ")
CR$Cited[1:10]
```

```
## CR
##    GARFIELD E    BORNMANN L       SMALL H      CRONIN B     GLANZEL W 
##           129            81            62            53            48 
##      WHITE HD    KOSTOFF RN       NARIN F LEYDESDORFF L    BROOKES BC 
##            48            45            41            40            38
```

The function *localCitations* generates the frequency table of the most local cited authors.
Local citations measures how many times an author included in this collection have been cited by other authors also in the collection.

To obtain the most frequent local cited authors:

```r
CR <- localCitations(M, results, sep = ".  ")
CR[1:10]
```

```
## CR
##      CRONIN B     GLANZEL W       NARIN F    SCHUBERT A    BAR ILAN J 
##            53            48            41            33            17 
##     VINKLER P       BRAUN T      HOLDEN G   OPPENHEIM C ARUNACHALAM S 
##            17            16            15            14            12
```


## Authors' Dominance ranking

The function *dominance* calculates the authors' dominance ranking as proposed by Kumar & Kumar, 2008.

Function arguments are: *results* (object of class *bibliometrix*) obtained by *biblioAnalysis*; and *k* (the number of authors to consider in the analysis).


```r
DF <- dominance(results, k = 10)
DF
```

```
##                        Dominance Factor Multi Authored First Authored
## KOSTOFF,RN                    1.0000000              8              8
## HOLDEN,G                      1.0000000              3              3
## ABRAMO,GIOVANNI               0.7500000              4              3
## GLANZEL,W                     0.7500000              4              3
## GARG,KC                       0.6666667              3              2
## MOPPETT,IK                    0.6666667              3              2
## BORNMANN,LUTZ                 0.6250000              8              5
## BORGMAN,CL                    0.3333333              3              1
## D'ANGELO,CIRIACOANDREA        0.2500000              4              1
## MARX,WERNER                   0.1666667              6              1
##                        Rank by Articles Rank by DF
## KOSTOFF,RN                            2          1
## HOLDEN,G                              9          2
## ABRAMO,GIOVANNI                       4          3
## GLANZEL,W                             6          4
## GARG,KC                               8          5
## MOPPETT,IK                           10          6
## BORNMANN,LUTZ                         1          7
## BORGMAN,CL                            7          8
## D'ANGELO,CIRIACOANDREA                5          9
## MARX,WERNER                           3         10
```

The Dominance Factor is a ratio indicating the fraction of multi authored articles in which a scholar appears as first author.

In this example, Kostoff and Holden dominate their research team because they appear as first authors in all their papers (8 for Kostoff and 3 for Holden). 

## Authors' h-index
The h-index is an author-level metric that attempts to measure both the productivity and citation impact of the publications of a scientist or scholar. 

The index is based on the set of the scientist's most cited papers and the number of citations that they have received in other publications.

The function *Hindex* calculates the authors' H-index and its variants (g-index and m-index) in a bibliographic collection.

Function arguments are: *M* a bibliographic data frame; *auhtors* a character vector containing the the authors' names for which you want to calculate the H-index. The aurgument has the form c("SURNAME1 N","SURNAME2 N",...). 

In other words, for each author: surname and initials are separated by one blank space. 
i.e for the authors ARIA MASSIMO and CUCCURULLO CORRADO, *authors* argument is *authors = c("ARIA M", "CUCCURULLO C")*.

To calculate the h-index of Lutz Bornmann in this collection:


```r
indices <- Hindex(M, authors="BORNMANN L", sep = ";",years=10)

# Bornmann's impact indices:
indices$H
```

```
##       Author h_index g_index   m_index TC NP
## 1 BORNMANN L       4       7 0.6666667 50  8
```

```r
# Bornmann's citations
indices$CitationList
```

```
## [[1]]
##                          Authors                        Journal Year
## 2      MARX WERNER;BORNMANN LUTZ SOZIALE WELT-ZEITSCHRIFT FUR S 2015
## 4 BORNMANN LUTZ;LEYDESDORFF LOET        JOURNAL OF INFORMETRICS 2014
## 8 BORNMANN LUTZ;BOWMAN BENJAMINF     ZEITSCHRIFT FUR EVALUATION 2012
## 3                  BORNMANN LUTZ            RESEARCH EVALUATION 2014
## 1      BORNMANN LUTZ;MARX WERNER        JOURNAL OF INFORMETRICS 2015
## 6 BORNMANN LUTZ;WILLIAMS RICHARD        JOURNAL OF INFORMETRICS 2013
## 7      BORNMANN LUTZ;MARX WERNER        JOURNAL OF INFORMETRICS 2013
## 5                  BORNMANN LUTZ JOURNAL OF THE AMERICAN SOCIET 2013
##   TotalCitation
## 2             0
## 4             1
## 8             2
## 3             3
## 1             5
## 6            10
## 7            11
## 5            18
```

To calculate the h-index of the first 10 most productive authors (in this collection):


```r
authors=gsub(","," ",names(results$Authors)[1:10])

indices <- Hindex(M, authors, sep = ";",years=50)

indices$H
```

```
##                    Author h_index g_index    m_index  TC NP
## 1           BORNMANN LUTZ       4       7 0.66666667  50  8
## 2              KOSTOFF RN       8       8 0.42105263 276  8
## 3             MARX WERNER       3       6 0.42857143  36  6
## 4              HUMENIK JA       5       5 0.27777778 213  5
## 5         ABRAMO GIOVANNI       4       4 0.44444444 158  4
## 6  D'ANGELO CIRIACOANDREA       4       4 0.44444444 158  4
## 7               GLANZEL W       2       5 0.08333333  64  5
## 8          ATKINSON ROGER       0       0 0.00000000   0  3
## 9                BARKER K       3       3 0.23076923  61  3
## 10             BORGMAN CL       3       3 0.10344828 225  3
```


## Lotka's Law coefficient estimation
The function *lotka* estimates Lotka's law coefficients for scientific productivity (Lotka A.J., 1926).

Lotka's law describes the frequency of publication by authors in any given field as an inverse square law, where the number of authors publishing a certain number of articles is a fixed ratio to the number of authors publishing a single article.
This assumption implies that the theoretical beta coefficient of Lotka's law is equal to 2.

Using *lotka* function is possible to estimate the Beta coefficient of our bibliographic collection and assess, through a statistical test, the similarity of this empirical distribution with the theoretical one.


```r
L <- lotka(results)

# Author Productivity. Empirical Distribution
L$AuthorProd
```

```
##   N.Articles N.Authors        Freq
## 1          1       483 0.884615385
## 2          2        42 0.076923077
## 3          3        14 0.025641026
## 4          4         3 0.005494505
## 5          5         1 0.001831502
## 6          6         1 0.001831502
## 7          8         2 0.003663004
```

```r
# Beta coefficient estimate
L$Beta
```

```
## [1] 3.073877
```

```r
# Constant
L$C
```

```
## [1] 0.6372572
```

```r
# Goodness of fit
L$R2
```

```
## [1] 0.9097339
```

```r
# P-value of K-S two sample test
L$p.value
```

```
## [1] 0.2031888
```

The table L$AuthorProd shows the observed distribution of scientific productivity in our example.

The estimated Beta coefficient is 3.05 with a goodness of fit equal to 0.94. Kolmogorov-Smirnoff two sample test provides a p-value 0.09 that means there is not a significant difference between the observed and the theoretical Lotka distributions.

You can compare the two distributions using *plot* function:


```r
# Observed distribution
Observed=L$AuthorProd[,3]

# Theoretical distribution with Beta = 2
Theoretical=10^(log10(L$C)-2*log10(L$AuthorProd[,1]))

plot(L$AuthorProd[,1],Theoretical,type="l",col="red",ylim=c(0, 1), xlab="Articles",ylab="Freq. of Authors",main="Scientific Productivity")
lines(L$AuthorProd[,1],Observed,col="blue")
legend(x="topright",c("Theoretical (B=2)","Observed"),col=c("red","blue"),lty = c(1,1,1),cex=0.6,bty="n")
```

<img src="bibliometrix-vignette_files/figure-html/Lotka law comparison-1.png" width="300px" />


## Bibliometric network matrices
Manuscript's attributes are connected to each other through the manuscript itself: author(s) to journal, keywords to publication date, etc.

These connections of different attributes generate bipartite networks that can be represented as rectangular matrices (Manuscripts x Attributes).

Furthermore, scientific publications regularly contain references to
other scientific works. This generates a further network, namely, co-citation or coupling network.

These networks are analysed in order to capture meaningful properties of the underlying research system, and in particular to determine the influence of bibliometric units such as scholars and journals.

### Bipartite networks
*cocMatrix* is a general function to compute a bipartite network selecting one of the metadata attributes.

For example, to create a network *Manuscript x Publication Source* you have to use the field tag "SO":


```r
A <- cocMatrix(M, Field = "SO", sep = ";")
```

A is a rectangular binary matrix, representing a bipartite network where rows and columns are manuscripts and sources respectively. 

The generic element $a_{ij}$ is 1 if the manuscript $i$ has been published in source $j$, 0 otherwise. 

The $j-th$ column sum $a_j$ is the number of manuscripts published in source $j$. 

Sorting, in decreasing order, the column sums of A, you can see the most relevant publication sources:


```r
sort(Matrix::colSums(A), decreasing = TRUE)[1:5]
```

```
##                                                         SCIENTOMETRICS 
##                                                                     49 
## JOURNAL OF THE AMERICAN SOCIETY FOR INFORMATION SCIENCE AND TECHNOLOGY 
##                                                                     14 
##                JOURNAL OF THE AMERICAN SOCIETY FOR INFORMATION SCIENCE 
##                                                                      8 
##                                                JOURNAL OF INFORMETRICS 
##                                                                      6 
##                                               JOURNAL OF DOCUMENTATION 
##                                                                      6
```



Following this approach, you can compute several bipartite networks:

* Citation network

```r
# A <- cocMatrix(M, Field = "CR", sep = ".  ")
```

* Author network

```r
# A <- cocMatrix(M, Field = "AU", sep = ";")
```

* Country network

Authors' Countries is not a standard attribute of the bibliographic data frame. You need to extract this information from affiliation attribute using the function *metaTagExtraction*.


```r
M <- metaTagExtraction(M, Field = "AU_CO", sep = ";")
# A <- cocMatrix(M, Field = "AU_CO", sep = ";")
```

*metaTagExtraction* allows to extract the following additional field tags: *Authors' countries* (`Field = "AU_CO"`); *First author of each cited reference* (`Field = "CR_AU"`); *Publication source of each cited reference* (`Field = "CR_SO"`); and *Authors' affiliations* (`Field = "AU_UN"`).

* Author keyword network

```r
# A <- cocMatrix(M, Field = "DE", sep = ";")
```

* Keyword Plus network

```r
# A <- cocMatrix(M, Field = "ID", sep = ";")
```

* Etc.

### Bibliographic coupling 

Two articles are said to be bibliographically coupled if at least one cited source appears in the bibliographies or reference lists of both articles (Kessler, 1963).

A coupling network can be obtained using the general formulation:

$$
B = A \times A^T
$$
where A is a bipartite network.

Element $b_{ij}$ indicates how many bibliographic coupling exist between manuscripts $i$ and $j$. In other words, $b_{ij}$ gives the number of paths of length 2, via which one moves from $i$ along the arrow and then to $j$ in the opposite direction.

$B$ is a simmetrical matrix $B = B^T$.

The strength of the coupling of two articles, $i$ and $j$ is defined
simply by the number of references that the articles have in common, as given by the element $b_{ij}$ of matrix $B$.

The function *biblioNetwork* calculates, starting from a bibliographic  data frame, the most frequently used coupling networks: Authors, Sources, and Countries.

*biblioNetwork* uses two arguments to define the network to compute:

* *analysis* argument can be "co-citation", "coupling", "collaboration",  or "co-occurrences".

* *network* argument can be "authors", "references", "sources", "countries", "universities", "keywords", "author_keywords", "titles" and "abstracts".

The following code calculates a classical article coupling network:

```r
# NetMatrix <- biblioNetwork(M, analysis = "coupling", network = "references", sep = ".  ")
```

Articles with only a few references, therefore, would tend to be more weakly bibliographically coupled, if coupling strength is measured simply according to the number of references that articles contain in common. 

This suggests that it might be more practicable to switch to a relative measure of bibliographic coupling.

*normalizeSimilarity* function calculates Association strength, Inclusion, Jaccard or Salton similarity among vertices of a network.


```r
NetMatrix <- biblioNetwork(M, analysis = "coupling", network = "authors", sep = ";")

# calculate jaccard similarity coefficient
S <- normalizeSimilarity(NetMatrix, type="jaccard")

# plot authors' similarity (first 20 authors)
net=networkPlot(S, n = 20, Title = "Authors' Coupling", type = "fruchterman", size=FALSE,remove.multiple=TRUE)
```

![](bibliometrix-vignette_files/figure-html/similarity-1.png)<!-- -->


### Bibliographic co-citation

We talk about co-citation of two articles when
both are cited in a third article. Thus, co-citation can be seen as the counterpart of bibliographic coupling.

A co-citation network can be obtained using the general formulation:

$$
C = A^T \times A
$$
where A is a bipartite network.

Like matrix $B$, matrix $C$ is also symmetric. The main diagonal
of $C$ contains the number of cases in which a reference is cited in our data frame. 

In other words, the diagonal element $c_{i}$ is the number of local citations of the reference $i$.

Using the function *biblioNetwork*, you can calculate a classical reference co-citation network:

```r
# NetMatrix <- biblioNetwork(M, analysis = "co-citation", network = "references", sep = ".  ")
```

### Bibliographic collaboration

Scientific collaboration network is a network where nodes are authors and links are co-authorships as the latter is one of the most well documented forms of scientific collaboration (Glanzel, 2004).

An author collaboration network can be obtained using the general formulation:

$$
AC = A^T \times A
$$
where A is a bipartite network *Manuscripts x Authors*.

The diagonal element $ac_{i}$ is the number of manuscripts authored or co-authored by researcher $i$.

Using the function *biblioNetwork*, you can calculate an authors' collaboration network:

```r
# NetMatrix <- biblioNetwork(M, analysis = "collaboration", network = "authors", sep = ";")
```

or a country collaboration network:

```r
# NetMatrix <- biblioNetwork(M, analysis = "collaboration", network = "countries", sep = ";")
```

## Visualizing bibliographic networks

All bibliographic networks can be graphically visualized or
modeled.

Here, we show how to visualize networks using function *networkPlot* and *VOSviewer software* by Nees Jan van Eck and Ludo Waltman (http://www.vosviewer.com).

Using the function *networkPlot*, you can plot a network created by *biblioNetwork* using R routines or using *VOSviewer*.

The main argument of *networkPlot* is type. It indicates the network map layout: circle, kamada-kawai, mds, etc.
Choosing type="vosviewer", the function automatically: (i) saves the network into a pajek network file, named "vosnetwork.net"; (ii) starts an instance of VOSviewer which will map the file "vosnetwork.net".
You need to declare, using argument *vos.path*, the full path of folder where VOSviewer software is located (es. vos.path='c:/software/VOSviewer').


### Country Scientific Collaboration


```r
# Create a country collaboration network

M <- metaTagExtraction(M, Field = "AU_CO", sep = ";")
NetMatrix <- biblioNetwork(M, analysis = "collaboration", network = "countries", sep = ";")

# Plot the network
net=networkPlot(NetMatrix, n = 20, Title = "Country Collaboration", type = "circle", size=TRUE, remove.multiple=FALSE)
```

![](bibliometrix-vignette_files/figure-html/Country collaboration-1.png)<!-- -->


### Co-Citation Network


```r
# Create a co-citation network

NetMatrix <- biblioNetwork(M, analysis = "co-citation", network = "references", sep = ".  ")

# Plot the network
net=networkPlot(NetMatrix, n = 15, Title = "Co-Citation Network", type = "fruchterman", size=T, remove.multiple=FALSE)
```

![](bibliometrix-vignette_files/figure-html/Co-citation network-1.png)<!-- -->

### Keyword co-occurrences


```r
# Create keyword co-occurrencies network

NetMatrix <- biblioNetwork(M, analysis = "co-occurrences", network = "keywords", sep = ";")

# Plot the network
net=networkPlot(NetMatrix, n = 20, Title = "Keyword Co-occurrences", type = "kamada", size=T)
```

![](bibliometrix-vignette_files/figure-html/Keyword c-occurrences-1.png)<!-- -->

## Co-Word Analysis: Conceptual structure of a field 
The aim of the co-word analysis is to map the conceptual structure of a framework using the word co-occurrences in a bibliographic collection.

The analysis can be performed through dimensionality reduction techniques such as Multidimensional Scaling (MDS) or Multiple Correspondence Analysis (MCA). 

Here, we show an example using the function *conceptualStructure* that performs a MCA to draw a conceptual structure of the field and K-means clustering to identify clusters of documents which express common concepts. Results are plotted on a two-dimensional map.

*conceptualStructure* includes natural language processing (NLP) routines (see the function *termExtraction*) to extract terms from titles and abstracts.  In addition, it implements the Porter's stemming algorithm to reduce inflected (or sometimes derived) words to their word stem, base or root form.



```r
# Conceptual Structure using keywords

CS <- conceptualStructure(M,field="ID", minDegree=4, k.max=5, stemming=FALSE, labelsize=10)
```

![](bibliometrix-vignette_files/figure-html/Co-Word Analysis-1.png)<!-- -->

## Historical Co-Citation Network
Historiographic map is a graph proposed by E. Garfield to represent a chronological network map of most relevant co-citations resulting from a bibliographic collection.

The function \code{histNetwork} generates a chronological co-citation network matrix which can be plotted using *histPlot*:


```r
# Create a historical citation network

histResults <- histNetwork(M, n = 10, sep = ".  ")

# Plot a historical co-citation network
net <- histPlot(histResults, size = FALSE,label=FALSE, arrowsize = 0.5)
```

![](bibliometrix-vignette_files/figure-html/Historical Co-citation network-1.png)<!-- -->

```
## 
##  Legend
## 
##                                                                                                   Paper
## 1986 - 1                                                                PERSSON O, 1986, SCIENTOMETRICS
## 1986 - 2                                                                          DEGLAS F, 1986, LIBRI
## 1987 - 3                                                               BROADUS RN, 1987, SCIENTOMETRICS
## 1987 - 4                                                      BROADUS RN, 1987, J  EDUC  LIBR  INF  SCI
## 1990 - 5                                                                PERITZ BC, 1990, SCIENTOMETRICS
## 1992 - 6                                                                       SENGUPTA IN, 1992, LIBRI
## 1994 - 7                                                  NARIN F;OLIVASTROD;STEVENSKA, 1994, EVAL  REV
## 2000 - 8                                                                         CRONIN B, 2000, J  DOC
## 2000 - 9                                                                WORMELL I, 2000, SCIENTOMETRICS
## 2001 - 10                                                        HOOD WW;WILSONCS, 2001, SCIENTOMETRICS
## 2006 - 11    DAIM TUGRULU;RUEDAGUILLENNO;MARTINHILARY;GERDSRIPISEK, 2006, TECHNOL  FORECAST  SOC  CHANG
## 2006 - 12                                   GLANZEL W;DEBACKEREK;THIJSB;SCHUBERTA, 2006, SCIENTOMETRICS
## 2008 - 13                                                              THELWALL MIKE, 2008, J  INF  SCI
## 2008 - 14                                                        SMITH DEREKR, 2008, CONTACT DERMATITIS
## 2009 - 15                 ABRAMO GIOVANNI;D'ANGELOCIRIACOANDREA;CAPRASECCAALESSANDRO, 2009, RES  POLICY
## 2011 - 16                                                    MOPPETT IK;HARDMANJG, 2011, BR  J  ANAESTH
## 2011 - 17                                   ABRAMO GIOVANNI;D'ANGELOCIRIACOANDREA, 2011, SCIENTOMETRICS
## 2011 - 18                                              MARX WERNER, 2011, J  AM  SOC  INF  SCI  TECHNOL
## 2011 - 19 D'ANGELO CIRIACOANDREA;GIUFFRIDACRISTIANO;ABRAMOGIOVANNI, 2011, J  AM  SOC  INF  SCI  TECHNOL
## 2013 - 20                                            BORNMANN LUTZ, 2013, J  AM  SOC  INF  SCI  TECHNOL
##                                        DOI Year LCS GCS
## 1986 - 1                10.1007/BF02016861 1986   3  15
## 1986 - 2         10.1515/LIBR.1986.36.1.40 1986   2   8
## 1987 - 3                10.1007/BF02016680 1987   5  38
## 1987 - 4                  10.2307/40323625 1987   2   2
## 1990 - 5                10.1007/BF02020148 1990   2   5
## 1992 - 6         10.1515/LIBR.1992.42.2.75 1992   4  20
## 1994 - 7        10.1177/0193841X9401800107 1994   5  62
## 2000 - 8          10.1108/EUM0000000007123 2000   3  20
## 2000 - 9           10.1023/A:1005688520197 2000   2   1
## 2001 - 10          10.1023/A:1017919924342 2001   2  71
## 2006 - 11   10.1016/J.TECHFORE.2006.04.004 2006   2 211
## 2006 - 12       10.1556/SCIENT.67.2006.2.8 2006   2  41
## 2008 - 13         10.1177/0165551507087238 2008   2  32
## 2008 - 14 10.1111/J.1600-0536.2008.01405.X 2008   2  11
## 2009 - 15     10.1016/J.RESPOL.2008.11.001 2009   3  43
## 2011 - 16               10.1093/BJA/AER124 2011   3  14
## 2011 - 17        10.1007/S11192-011-0352-7 2011   2  35
## 2011 - 18                10.1002/ASI.21479 2011   2  17
## 2011 - 19                10.1002/ASI.21460 2011   2  64
## 2013 - 20                10.1002/ASI.22792 2013   3  18
```

## Main Authors' references (about bibliometrics)

Cuccurullo, C., Aria, M., & Sarto, F. (2016). *Foundations and trends in performance management. A twenty-five years bibliometric analysis in business and public administration domains*, **Scientometrics**, DOI: 10.1007/s11192-016-1948-8 (https://doi.org/10.1007/s11192-016-1948-8).


Cuccurullo, C., Aria, M., & Sarto, F.  (2015). *Twenty years of research on performance management in business and public administration domains.* Presentation at the **Correspondence Analysis and Related Methods conference (CARME 2015)** in September 2015 (http://www.bibliometrix.org/documentation/2015Carme_cuccurulloetal.pdf).


Sarto, F., Cuccurullo, C., & Aria, M. (2014). *Exploring healthcare governance literature: systematic review and paths for future research.* **Mecosan** (http://www.francoangeli.it/Riviste/Scheda_Rivista.aspx?IDarticolo=52780&lingua=en).


Cuccurullo, C., Aria, M., & Sarto, F. (2013). *Twenty years of research on performance management in business and public administration domains.* In **Academy of Management Proceedings** (Vol. 2013, No. 1, p. 14270). Academy of Management (https://doi.org/10.5465/AMBPP.2013.14270abstract).
