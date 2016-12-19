# FFP
Feature Frequency Profile(FFP) two core programs


## Requirements  
- GCC(g++) version 4.7.1+  
- Google sparse hash library. Look here: https://github.com/sparsehash/sparsehash  
- zlib version 1.2.8+. Look here: http://www.zlib.net/  


## 1. FFP_compress.cpp
Compile: g++ -std=c++11 -o (execute name) (this script) -lz  
Run example: [Program path][options][input file path][output file path]  

### [Arguments]
* -h  
    Show options  
* -s [INT]  
    Feature size (l-mer)  
* -e [INT]  
    Feature size (l-mer) range end. Functional only when measuring vocabulary size using with -V  
* -a  
    Takes 20 amino acids sequences as inputs  
* -c  
    Convert and accept nucleotide bases into RY bases 
* -k [STR]  
    Manual input of alphabet bases string. For example input 'HJKL' is ['H', 'J', 'K', 'L'] bases  
* -r  
    Disable reverse complement counting. Any bases set other than AGCT code will disable reverse complement counting  
* -n  
    Output frequency into ratio. A feature count / Total feature count  
* -u  
    Accept masked letters which are lower cases. FASTA format generally represent masked bases into lower cases  
* -V  
    Measure vocabular size at given range of feature length (l-mer)  
* -b [LONG LONG]  
    Bottom limit. Remove any feature that counts less than -b  
    Default = 1
* -t [LONG LONG]  
    Top limit. Remove any feature that counts larger than -t  
    Default = 0 = maximum  
    

### [Note]
Using [-a], amino acids, automatically turn [-r], disable reverse complement counting, becase peptide sequence have direction(start code -> stop codon), however, nucleotide(genome) sequence actually is double helix that has reverse complement strand.


Reverse complement counting actually do picking one 'forward' or 'backward' feature of sequence that is lexically prior than another, because couting number of all forward and backward feature is equal to reverse complement counting x 2.

For example,
In DNA double helix,

AAAATTT -> lexically prior, so pick this one  
TTTTAAA -> even if actual readen feature is this!  


### [Input]
FASTA format peptide or nucleotide sequence files. 


### [Output]
Data compressed Feature Frequency Profile


## 2. JSD_matrix.cpp
Compile: g++ -std=c++11 -pthread -o (execute name) (this script) -lz  
Run example: [Program path][options][input files path] > [output file path(standard output)]  

### [Arguments]

* -h  
    Show options  
* -c [INT]  
    Specific integer code of single or escape character for delimiter in bewteen a set of [Feature|Value]  
    Default is 13, '\n', linebreak
* -t [INT]  
    Number of threads for multiprocessing  
    Heuristically, an adequate thread number is the number of cpu cores - 1. Default is 5
* -r [PATH]  
    Input previous matrix, and add more items to the matrix without calculating a whole    
* -d  
    Output Jensen-Shannon distance matrix instead of Jsensen-Shannon divergence matrix which is default  
    Square root(JS divergence) = JS distance  
    

### [Note]
[-r] input low triangular matrix, and it requires all pair-wise FF_Profiles


### [Input]
The output files of 'FFP_compress' which are Feature Frequency Profile(FFP)s


### [Output]
Standard output of low triangular Jensen-Shannon divergence, or distance, matrix


## Limitation:
Generally, longer feature lengths(l-mer) consume more memory and time.  
So far vocabulary size up to 20(amino acids) ^ 24(feature length) in fungi proteomes study, maximum 35,274 proteins of 10,866,611 amino acids, was used and worked. Although 20^24 is huge number, actual vocabular size is much smaller in non-random sequences such as peptide sequences.
