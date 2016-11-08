# FFP
Feature Frequency Profile(FFP) two core programs


Prerquirement
A. GCC(g++) version 4.7.1+  
B. Google sparse hash library. Can be download here: https://github.com/sparsehash/sparsehash  
C. zlib version 1.2.8+. Can be download here: http://www.zlib.net/  


1. FFP_compress.cpp
Compile option: g++ -std=c++11 -o (execute name) (this script) -lz
Run example: [Program path][options][input file path][output file path] 

[options(parameters)]
-h  show help, show options

-s  [INT] feature size

-a  take amino acids sequence

-c  convert and accept nucleotide(AGCT) code to RY code

-k  [STR] manual input of word alphabets: For example input 'HJKL' as ['H', 'J', 'K', 'L'] set

-r  disable reverse complement counting

-n  output ratio instead of frequency

-u  letter case insensitive(default is case sensitive)

-V  brief version(or function)
    count the number of features, those frequency limit by [-b] and [-t]

-b  [LONG LONG] bottom limit of feature frequency in vocabulary size measure
    default = 2

-t  [LONG LONG] top limit of feature frequency in vocabulary size measure
    default = 0 = maximum


[note]
Using [-a], amino acids, automatically turn [-r], disable reverse complement counting, becase peptide sequence have direction(start code -> stop codon), however, nucleotide(genome) sequence actually is double helix that has reverse complement strand.


[input]
FASTA format peptide or nucleotide sequence files. 


[output]
Data compressed Feature Frequency Profile




2. JSD_matrix.cpp
Compile option: g++ -std=c++11 -pthread -o (execute name) (this script) -lz
Run example: [Program path][options][input files path] > [output file path(standard output)]


[options(parameters)]
-h  show help, show options

-c  [INT] specific integer code of single or escape character for delimiter
          default = TAB = char(9), integer 9
          
-t  [INT] set a number of thread for calculation(multiprocessing)
          adequate thread number is #of_cpu_cores - 1. Default is 1?
          
-r  [PATH]  Input reserved, previous, distance matrix
            Useful when adding more items without calculating prior pairs, thus saves overall time.
       
       
[note]
[-r] input accept low triangular distance matrix, and it requires all pair-wise FF_Profiles


[input]
the output files of 'FFP_compress' which are Feature Frequency Profile(FFP)s


[output]
Standard output of low triangular distance matrix


Limitation:
Generally, longer feature lengths(l-mer) consume more memory and time.
So far vocabulary size up to 20(amino acids) ^ 24(feature length) was used and tested.
