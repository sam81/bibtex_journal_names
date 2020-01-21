This repository contains lists of long and abbreviated versions of journal names that can be used to mantain bibtex and biblatex databases.

Bibtex and biblatex databases have a `journal` entry that defines the journal name for `@article` types. Unfortunately there is not a built-in mechanism to define both the long and abbreviated versions of journal names. As a result, if you want to support both styles that need the long, and styles that need the abbreviated version of a journal title you're forced to maintain two databases.

However, there's workaround to make the above task easy. On top of your `.bib` file you can declare a series of `@string` variables to be substituted with a string. For example, if you put the following two lines on top of your bibtex database:

    @string{JAcoustSocAm="The Journal of the Acoustical Society of America"}
    @string{HearRes="Hearing Research"}
	
and use the variable names (e.g. `JAcoustSocAm`, `HearRes`) instead of the journal names in your entries, e.g.:

    @Article{Viemeister1979,
       Author="Viemeister, N. F. ",
       Title="{{T}emporal modulation transfer functions based upon modulation thresholds}",
       Journal=JAcoustSocAm,
       Year="1979",
       Volume="66",
       Number="5",
       Pages="1364--1380",
       doi="10.1121/1.383531"
     }
	 
when you run bibtex or biber the variables will be substituted with the journal names. Note that in order to support both styles using long and abbreviated journal names you will still need to maintain two different databases, one with the `@string` variables associated with the long journal names, and one with the `@string` variables associated with the abbreviated journal names. However, the process can be largely automated: 1) build one database *using* the variables in the bibtex entries, 2) write two text files, one with the long, and one with the abbreviated journal names, and 3) run a script that pastes the contents of either of these two text files at the beginning of the database file to obtain the database with the desired journal name style. 

This repository provides two text files with the `@string` variables for a number of journals in the long (`subs_long.txt`), and in the short (`subs_short.txt`) journal name version. The shell script `mkdbs.sh` collects bibtex entries from user-written bibtex files ordered alphabetically (`A.bib`, `B.bib`, etc...), pastes them together, and puts the content of the `subs_long.txt` and `subs_short.txt` files at the top to generate a `refs_long.bib`, and `refs_short.bib` file that are ready to use for projects that respectively need the long, and projects that need the abbreviated journal names. The user-written bibtex files need to be in a "sibling" folder named `bibtex_database` (but this can be easily changed by modifying the `mkdbs.sh` file if desired), and the `refs*` files will also be generated in this sibling folder.

Currently the list of journal names includes mostly journals in the hearing sciences, neuroscience, and psychology. Contributions to expand the journal list are welcome. The variable names used are always the abbreviated journal names without spaces or dots. Please make a pull request if you want to contribute.
