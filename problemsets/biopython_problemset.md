# Biopython problem set

## Customizing your VS Code

Ask TAs about changing your python version in VS Code to get VS code to recognize the BioPython installation.      
Ask TAs about installing VS Code Python extentions.   

## FASTA Parser

1. Create a new FASTA parser that uses BioPython SeqIO (e.g. `from Bio import SeqIO`) ([review `SeqIO.parse` in notes](../lectures/biopython.md#read-a-fasta-file)) to print the sequence name, description, and sequence as tab delimited output
2. Add in some code to print out stats about your FASTA records in your multi-FASTA file:
   -  total number of sequences
   -  total number of nucleotides
   -  average length of sequences
   -  shortest sequence length
   -  longest sequence length
   -  average GC content of all the sequences in the file
   -  the sequence with the highest GC content
   -  the sequence with the lowest GC content
  ```
  sequence count: ? 
  total number of nucleotides: ? 
  avg len: ? 
  shortest len: ? 
  longest len: ? 
  avg GC content: ? 
  lowest GC content: ? 
  highest GC content: ?

  ```
You can use code you have previously written to calculate GC content, or try the Bio.SeqUtils gc_fraction function to calculate it. 

gc_fraction will work with Bio.SeqRecord.SeqRecord objects as well as a literal DNA sequence string(seq_record)


gc_fraction using a String
```python
>>> from Bio.SeqUtils import gc_fraction
>>> gc_fraction('GGCCTTTTTTTT')
0.3333333333333333
```

gc_fraction using a seq_record
```python
>>> from Bio import SeqIO
>>> from Bio.SeqUtils import gc_fraction
>>> filename = "seq.nt.fa"

>>> filename = "seq.nt.fa"
>>> for seq_record in SeqIO.parse(filename,"fasta"):
...     print(seq_record.id,gc_fraction(seq_record))
...
...
seq1 0.46111111111111114
seq2 0.5222222222222223
seq3 0.25510204081632654
seq4 0.37799043062200954
```

  
3. Test your code with a small test set of 2 or 3 very short sequences.
4. Run your code on [Python_08.fasta](../files/Python_08.fasta)



## Running BLAST with your own custom database



These questions will take some research and set up. Spend some time reading about how to run blast and ask for help as needed. We are going to make a custom database for BLAST. This database will contain the proteins from Salmonella paratyphi B which you will create in the following steps:

1.  Download uniprot_sprot using the Unix command 'curl':

```
curl -OL ftp://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/complete/uniprot_sprot.fasta.gz
```
**Make sure to not add this to your GitHub Repository. It is tooooo big and with cause problems**

2. Unzip the file using the Unix command 'gunzip':

```
gunzip uniprot_sprot.fasta.gz
```
This will create a file `uniprot_sprot.fasta`

**Do not add uniprot_sprot.fasta to your github repo. It is too big.*** To be safe, find your .gitignore in the root of your github repository. Add `uniprot_sprot.fasta*` anywhere in the file. Make sure to add this file to our index as you are updating your repo.

Here is a way to ENSURE that you don't mistakenly commit a large file. Get help from TA if you do not know where your .git directory is. You might have already completed this on day 1 in the Unix exercises.
```
cd .git/hooks/
curl -OL https://raw.githubusercontent.com/prog4biol/pfb2025/master/setup/pre-commit
```


3. What does the uniprot_sprot.fasta file contain? How many records? Does it look intact? How do you know? Try using command line functions like "grep" and "wc -l" to determine this rather than python

### Extracting Salmonella paratyphi B entries from `uniprot_sprot.fasta`

1. With the `Bio.SeqIO` module, read in `uniprot_sprot.fasta`

2. Print out and look at the descriptions of each record (seq_record.description). In this particular file, the description field almost always has a field OS=... that includes a species or strain designation. Here's an example

```
sp|A9N862|AAEB_SALPB p-hydroxybenzoic acid efflux pump subunit AaeB OS=Salmonella paratyphi B (strain ATCC BAA-1250 / SPB7) GN=aaeB PE=3 SV=1
```

Here the genus is _Salmonella_ and the species is _paratyphi_. There is also a strain 'B (strain ATCC BAA-1250 / SPB7). You can ignore this part. 

3. Make a new fasta file of all the sequences containing the species 'Salmonella paratyphi B'. Include the 'B' for this part of the exercise. Call this protein file `s_paratyphi.prot.fa`. You'll want to loop through all the sequence records, extract the description, find matches to 'Salmonella paratyphi B' and convert to fasta.


__Install NCBI Blast+__
1. Install NCBI Blast using conda from bioconda
```
conda install -c bioconda blast
```

OR (just so you  know) -- Don't do both -- you can download and unpackage the [ncbi-blast-2.17.0+-x64-macosx.tar.gz](https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/ncbi-blast-2.17.0+-x64-macosx.tar.gz) all the NCBI-BLAST tools like, blastp. 


2. Now, you will have the blast executables available. (blastn, blastx, tblastn, tblastx, blastp, makeblastdb, blastdbcmd), try a new terminal if they do not work


1. Run `makeblastdb -dbtype prot -in s_paratyphi.prot.fa -parse_seqids` to create a custom BLAST database containing the Salmonella proteins. Run `makeblastdb -help` for more info on this command
2. After you make your BLAST db with s_paratyphi.prot.fa, try to BLAST a protein such as [purH](https://raw.githubusercontent.com/prog4biol/pfb2025/master/files/purH.aa.fa) against the S. paratyphi B proteins. You can use the `blastp -db s_paratyphi.prot.fa -query purH.aa.fa -outfmt 5 -out result.xml`. The BLAST XML contains very detailed output about the alignments. Note that you can alternatively use -output 6 for a more human readable tab separated output format, but for BioPython we will use the XML output format (-outfmt 5)

      
__Parse BLAST Output__


1. Use BioPython to parse your XML BLAST results. You can refer to the code in the lecture notes for this [in the lecture notes](../lectures/biopython.md#parsing-blast-output). Print out all the hit sequence ID that are better than 1e-5 as well as their descriptions in tab separated columns.
2. Print the E-value and the score and the length of the alignment and the % similarity (not % identity)




__General instructions for using BLAST+__
1. First format you FASTA file so that BLAST+ can use it as a database
  `makeblastdb -in [FASTAFILE] -dbtype [nucl or prot] -parse_seqids`
      - `-in` is the switch for the FASTA formatted sequence file that you want to use as your BLAST db
      - `-dbtype` needs to be `prot` or `nucl`. This has to correspond to the sequence type in your FASTA file
      - `parse_seqids` makes it possible for you to retrieve individual sequences by name using `blastdbcmd`
2. Run `blastp -help` for information about running the command and formatting output.
3. Run `blastp -query [Your Query FASTA File] -db [BLAST FORMATTED DB FASTA FILE] -out [output file name] -evalue [evalue cutoff] -outfmt [5 for XML; 6 for TAB; etc]`
      - `-query`  A FASTA formatted sequence file with one or more query sequences
      - `-db` The file name of the FASTA formatted file you formatted with `makeblastdb`
      - `-out` A name of your choice for your output file, otherwise, the output is printed to the screen
      - `-evalue` The Expectation value (E) threshold for returning hits. 1e-5 is a common cutoff (Bill will say 1e-2, but we will be a tad more conservative)
      - `-outfmt` Choose the output format of your BLAST report as XML(5) `-outfmt 5` .  TAB(6) is also common output but unparsable by BioPython.  

  
