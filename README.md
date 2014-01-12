RefManageR
========

R package RefManageR

Overview
=====================================
The purpose of this work is to develop an R package which will make it very easy for users to manipulate bibliographic information within R.  R already has a useful `bibentry` class and `person` class for managing `BibTeX` entries and author information.  There is also a very useful function in the package `bibtex` for reading a `.bib` file into and creating an object of class `bibentry` from the references in the file.  

We will add additional functionality to these existing tools in a number of ways: 1) By including many additional tools for importing bibliographic references, 2) By providing convenient tools for manipulating objects of class `bibentry` including searching and merging, 3) By providing tools for summarizing `bibentry` objects including plotting, printing, tabling, writing to files or to the web, etc., and 4) By providing support for `BibLaTeX` which is to this point not available in R, but offers much, much greater functionality for using `.bib` files in `LaTeX` than `BibTeX`.

To Do:
==================================================================================================================
Important
-------------------
23. create bibstyle for biblatex
25. dig around in `utils:::citeNatbib`
1. new open function; convert all other RefManager.R functions to work with bibentry class
4. write to zotero: http://www.zotero.org/support/dev/server_api/v2/write_requests
5. boolean for how invalid entries are handled: either change to misc or drop (see what force=TRUE does in bibentry fnctn)
8. plot function. sig.: plot(db, field, plottype, ...)  both 'author' and 'author-full' possible          
  * See igraph pkg or perhaps adapt http://bl.ocks.org/mbostock/3037015 (add counts on edges)
  * See the [wordcloud package](http://blog.fellstat.com/?cat=11)
9. make table function.  table(db, field)  both 'author' and 'author-full'    
10. change the print.bibentry function for when style='bibtex' and add style='biblatex'  
  * (i.e. convert biblatex to bibtex for submitting to journal.  
  * i.e. change toBibtex function and add toBibLaTeX function)
[S/O thread explaining how to do this](http://tex.stackexchange.com/questions/114787/converting-from-biblatex-to-bibtex-format-using-biber)  
have check=FALSE argument in print.BibEntry if user does not want to check proper format when printing biblatex or bibtex  
have printonly=FALSE argument in toBibtex.BibEntry and toBibLatex.BibEntry if user does not want converted BibEntry object returned - see `utils:::toBibtex.bibentry`
11. fix print to handle date field and _requires adding a bibstyle_
12. implement a useful summary function
16. update print function to include index (i.e. entry numbers) in database
67. Convert to and from data.frame for use with plyr
592. read from WorldCat   
33. Read bibentries from clipboard
98. duplicated() and anyDuplicated() function - use as.data.frame
2. UpdateSingleAuthor function
34. Make sure all functions use Date class, make sure print and format truncate to year

### Optional Additional Ideas
* Make sure unicode handled properly? Unicode_alphabetic_tokenizer function in Unicode pkg  
Also see ?Encoding and ?iconv.  biblatex to bibtex function should use `iconv`
* http://opencitations.net/
* handle crossrefs for new BibLaTeX "in" fields
* handle xdata in BibLaTeX and entry sets in BibTeX and BibLaTeX
  * see 3.11.6 and 3.11.5, respectively in BibLaTeX manual
* write "-.BibEntry" function
* see the `scholar` R package.  Imports bibtex data using scholar id's
* see R package CITAN for "scientometrics"
* see knitcitations: install_github("knitcitations", "cboettig")
* Support for additional features for BibEntry objects
  2. xref entry type
  6. xdata
  1. mkbibquote
  60. related, relatedtype, relatedstring, relatedoptions p. 26
  9. increased crossref support
  7. support for set entry type (for citing multiple references with one key)
  13. support for pagination and bookpagination fields (predefined: page, column, line, verse, section, paragraph)
  15. BibLaTeX manual p. 29 Section 2.3 Usage Notes.
  18. **All the fancy sorting**
* handle Zotero using techreport for arXiv files
* add merge function as wrapper for "+.BibEntry"
* add search function as wrapper for "[.BibEntry"
* is [`stringi`](http://docs.rexamine.com/R-man/stringi/stringi-encoding.html) worth using over `Unicode` package?
* Distinguish what can be imported and needs to be re-implemented from `bibtex` and `utils`
* efficient ways to update database with only recently added papers, mention in JSS manuscript
* read all of R Journal paper on person and bibentry classes
* OR in search function - split
* discuss methods(class='bibentry') in JSS manuscript
* add saved search parameter to ReadZotero function
* summary function
* add option for sorting results of subsetting based on diff fields and printing. See: utils:::sort.bibentry
* add ORCHID to person class
* Integrate with: http://ropensci.org/packages/
* add 'html' and 'html5' option to print function
  * Check out [R2HTML](http://cran.r-project.org/web/packages/R2HTML/R2HTML.pdf)
  * consider  option for html or html5 (if html5 use outline+summary to separate years)
  * option for numbering ( < ul > or < ol > )
  * option for download pdf or display url
  * option link.text='Download'
  * handle eprints

BUGS
=================================================================================
* merge doesn't work when `length(bib1) > length(bib2)` in `bib1+bib2`
- (**FIXED**) fix tryCatch in MakeBibEntry 
- (**FIXED**) `$` doesn't work for creating field that does not exist in any entries
  * happened in ReadPDFs when adding file info after calling MakeCitationList
  * possible problem when list elements sent to make citation list are already BibEntry class?
* if doi + url both available, `print` function formats and includes both as url
* ReadPDF  
  - (**FIXED**) key needs to be made after scanning text and meta data in ReadPDFs, not by both separately  
  - (**FIXED**) msgs for entries with no author+title should not occur in both reading of text and metadata in `ReadPDFs` 
  * bug in adding `file` to `bibentry` object in ReadPDFs
  * handle ligatures in ReadPDFs: ff: "< U + F B 0 0 >" 
  - (**FIXED**) volume and number not working
  * (**FIXED**) files[[13]] - BoveHeld-BFapproximation(ArXiv2013).pdf should work for both authors - only gets one
  - year for WoodKohnShivelyJiang-BayesSSselection(JRSSB2002).pdf in GetJSTOR
  - (**FIXED**) handle results from CrossRef being list of BibEntry obj, instead of BibEntry obj. itself
  * add DOI's and file names at end
  * make sure all corner cases work: no crossref, no c.ref results, no metadata, no meta res, no JSTOR res,
    all jstor, all crossref+jstor
  * catch errors in MakeBibEntry
  * handle headers better when reading pdf, including getting year, etc. from 2nd line of doc
- (**FIXED**) `$<-`: `bib.entry.obj$field.name <- value.vec` adds vector of values to each bib. entry
- (**FIXED**) testbib <- ReadBib(system.file("REFERENCES.bib", package="bibtex")) fails, but read.bib works
- `bib[numeric_index, 'field_name']` or `bib['field_name', numeric_index]` not working
- warnings in ReadBib
- handling duplicates authors when printing in "authoryear" format (`extrafield` in BibLaTeX)
- Don't duplicate printing of Date if urldate or eventdate but no date in entry
- (**FIXED**) '\u00a7' encoded incorrectly for fmtPages, for some reason correct for fmtTotalPages
- authoryear bibstyle date, pages and title, totalpages

DONE     
==================================================================================================================

* read from Zotero: http://www.zotero.org/support/dev/server_api/v2/read_requests
* get bib entries from Google Scholar by scholar ID
* Can get DOIs or BibTeX entries from CrossRef
* Read PDFs to get bib information
* `BibLaTeX` entry types and fields supported - still needing printing and conversion
* Unicode can be supported with little effort. encoding option in WriteBib; Unicode pkg; Encoding function
* use unclass(bibentryObj) to get entry type and key
* made date field date object
* Extract operator works with date
* created names function to return keys of multiple entries or fields of one entry
* Extract function to takes list argument
* Extract function to takes vector of keys
* Exract works with entry types
* `$<-` works to assign fields from _bibentry_ class
* levels.BibEntry to access fields
* added unlist and relist functions. relist use somewhat odd.
* let users set "[.BibEntry" and "+.BibEntry" defaults using getOptions() or BibEntryOptions() accessors and mutators
* add function for merging which handles duplicates (needs debugging)
* allow vector fields in search function `all(pmatch(searchterm, fields))` perhaps?
* `]]<-` for replacing single BibEntries
* `]<-` for for updating (multiple) fields of (multiple) entries
* search PubMed and import Bibtex entry, get Pubmed ID from PubMed
* open BibEntry using file, doi, url, or eprint
- @strings for shortcut for commonly used strings (*Implemented already for bibentry*)

=============================================================================================================

JSS Doc Outline
================================
1. **Intro**
2. **Importing References**
  1. ReadBib
  2. ReadText
  2. ReadZotero
  3. ReadGS
  4. ReadCrossRef
  5. ReadPDFMeta
  7. ReadPubMed
3. **Manipulating BibEntry Objects**
  1. Searching and Indexing
  2. Assignment
  2. as.BibEntry
    * unlist, relist.BibEntry
    * unclass
    * as.data.frame.BibEntry
  2. Merging
  3. Other Generics - sort, names, $, levels, 
4. **Printing/Viewing/Saving**
  1. Already available in bibentry
  3. toBiblatex - toBibtex
  98. Opening
  43. WriteBib? or just write.bib from `biblatex`
  23. WriteZotero
5. **Summarizing**
  1. Plot
    1. word diagrams
    2. d3.js directed graph with and without centre
  2. `table`
