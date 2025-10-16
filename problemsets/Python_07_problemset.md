Python 7 - Regular Expressions - Problem Set
===================

1. In the file [Python_07_nobody.txt](https://raw.githubusercontent.com/prog4biol/pfb2025/master/files/Python_07_nobody.txt) find every occurrence of 'Nobody' and print out the position.

2. In the file [Python_07_nobody.txt](https://raw.githubusercontent.com/prog4biol/pfb2025/master/files/Python_07_nobody.txt) substitute every occurrence of 'Nobody' with your favorite name and write an output file with that person's name (ex. Michael.txt).

3. Using pattern matching, find all the FASTA header lines in [Python_07.fasta](https://raw.githubusercontent.com/prog4biol/pfb2025/master/files/Python_07.fasta). Note that the format for a header in a FASTA file is a line that starts with a greater than symbol and is followed by some text (e.g. `>seqName description` where seqName is the sequence name or identifier. The identifier cannot have spaces in it. The description that follows it can have spaces.)

4. If a line matches the format of a FASTA header, extract the sequence name and description using sub patterns (groups). 
	- Print sequence information in this format: `id:seqName desc:seqDescription`

5. Create a FASTA parser, or modify your FASTA parser from the previous problem set, to use regular expressions. Also make sure your parser can deal with a sequence that is split over many lines.

6. The enzyme ApoI has a restriction site: R^AATTY where R and Y are degenerate nucleotides. See the [IUPAC table](https://www.bioinformatics.org/sms/iupac.html) to identify the nucleotide possibilities for the R and Y. Write a regular expression to find and print all occurrences of the site in the following sequence [Python_07_ApoI.fasta](https://raw.githubusercontent.com/prog4biol/pfb2025/master/files/Python_07_ApoI.fasta). 

```
>seq1
GAATTCAAGTTCTTGTGCGCACACAAATCCAATAAAAACTATTGTGCACACAGACGCGAC
TTCGCGGTCTCGCTTGTTCTTGTTGTATTCGTATTTTCATTTCTCGTTCTGTTTCTACTT
AACAATGTGGTGATAATATAAAAAATAAAGCAATTCAAAAGTGTATGACTTAATTAATGA
GCGATTTTTTTTTTGAAATCAAATTTTTGGAACATTTTTTTTAAATTCAAATTTTGGCGA
AAATTCAATATCGGTTCTACTATCCATAATATAATTCATCAGGAATACATCTTCAAAGGC
AAACGGTGACAACAAAATTCAGGCAATTCAGGCAAATACCGAATGACCAGCTTGGTTATC
AATTCTAGAATTTGTTTTTTGGTTTTTATTTATCATTGTAAATAAGACAAACATTTGTTC
CTAGTAAAGAATGTAACACCAGAAGTCACGTAAAATGGTGTCCCCATTGTTTAAACGGTT
GTTGGGACCAATGGAGTTCGTGGTAACAGTACATCTTTCCCCTTGAATTTGCCATTCAAA
ATTTGCGGTGGAATACCTAACAAATCCAGTGAATTTAAGAATTGCGATGGGTAATTGACA
TGAATTCCAAGGTCAAATGCTAAGAGATAGTTTAATTTATGTTTGAGACAATCAATTCCC
CAATTTTTCTAAGACTTCAATCAATCTCTTAGAATCCGCCTCTGGAGGTGCACTCAGCCG
CACGTCGGGCTCACCAAATATGTTGGGGTTGTCGGTGAACTCGAATAGAAATTATTGTCG
CCTCCATCTTCATGGCCGTGAAATCGGCTCGCTGACGGGCTTCTCGCGCTGGATTTTTTC
ACTATTTTTGAATACATCATTAACGCAATATATATATATATATATTTAT
```


7. Determine the site(s) of the physical cut(s) by ApoI in the above sequence. Print out the sequence with "^" at the cut site.

  Hints:  
   - Use `sub()`  
   - Use subpatterns (parentheses and `group()` ) to find the cut site within the pattern.
   - Example: if the pattern is GACGT^CT the following sequence

```
AAAAAAAAGACGTCTTTTTTTAAAAAAAAGACGTCTTTTTTT
```
we want to display the cut site like this:

```
AAAAAAAAGACGT^CTTTTTTTAAAAAAAAGACGT^CTTTTTTT
```

8. Now that you've done your restriction digest, determine the lengths of your fragments and sort them by length (in the same order they would separate on an electrophoresis gel).

Hint: Convert this string:

```
AAAAAAAAGACGT^CTTTTTTTAAAAAAAAGACGT^CTTTTTTT
```

Into this list:

```
["AAAAAAAAGACGT","CTTTTTTTAAAAAAAAGACGT","CTTTTTTT"]
```

9. Download this file: ftp://ftp.neb.com/pub/rebase/bionet.txt or download from [our github](https://raw.githubusercontent.com/prog4biol/pfb2025/master/files/bionet.txt). This file is contains a list of enzymes and their cut sites. Create a script that reads this file to fill a dictionary with the enzymes paired with their recognition patterns. Skip the top 10 header lines and be aware of how the columns are delimited. You'll modify this script in the next question.

10. Modify your last script to take two command line arguments: the name of an enzyme and a fasta file with a sequence to be cut. Load a dictionary of enzyme names and cut sites from the code you developed in question 9.
   If the enzyme is present in the dictionary, and can cut the sequence, print out:
     - the sequence, annotated with cut sites
     - the number of fragments
     - the fragments in their natural order (unsorted)
     - the fragments in sorted order (largest to smallest)
     
__OPTIONAL__    
    
11. Search for every cut site found in the bionet collection of restriction enzymes in a user provided FASTA file. For each sequence report the 
   - sequence id
   - enzyme name
   - number of fragments
   - average fragment length
   - max fragment length
   - min fragment length

