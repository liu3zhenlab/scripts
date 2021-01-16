# Primer ePCR
### Overview
The script takes a fasta file of primers and aligns each to a reference genome for all alignments allowing a few mismatches. With all the alignments, pairs of primers mapped to a certain region of a reference sequence (typically a chromosome) are identified and output.

### Dependency
[Perl](https://www.perl.org/)  
[bowtie](http://bowtie-bio.sourceforge.net)

### Data requirement
1. a primer fasta file, see example in [primer.fas](primer.fas)
2. a fasta file of a reference

### Usage
```
Usage: primer.ePCR.pl [options]  
Options:  
  -p str  primer fasta file; required  
  -r str  bowtie index database; required  
  -m num  PCR min length (50)  
  -s num  PCR max length (10000)  
  -t num  maximum mismatches, must <=4 (3)  
          the penalty is 2 for the first 3 bases at 3' end   
  -b str  bowtie parameters, refer to bowtie-bio.sourceforge.net (-p 4 -B 1 -n 1 -y -v 2 -a -l 10 -f --best --quiet --sam-nohead)  
```

### Example
Here is an example that shows how to check primers in [primer.fas](primer.fas) on a reference genome.

First, the reference genome needs to be indexed.
```
bowtie-build <path-to-reference_fasta> <path-to-bowtie-database>
```

Second, run the script with the inputs and a primer fasta file and a Bowtie indexed database (e.g., <path-to-bowtie-database>) 
```
#!/bin/bash
pfas=primer.fas
ref=<path-to-bowtie-database>
prefix=primer.onRef
perl primer.ePCR.pl -p $pfas -r $ref \
  1>${prefix}.txt 2>${prefix}.log
```

In the [output](primer.onRef.txt), each pair islabeled to be uniquely or multiply mapped and the amplicon information is provided.
```
unique	G1_R	GTACAGACCCGCGACTATGA	0	+	6	73401719	G1_F	GTTGGAGCTGTGTCAAACGT	0	-	6	73402450	732
unique	G2_R	TGTGTCTTTGTCAGCAGCTC	0	+	2	244412182	G2_F	TTGCTTGTTCCGGGAGATCT	0	-	2	244412620	439
unique	G3_R	TGGTCACTGCTCAACAAGGT	0	+	9	65017072	G3_F	TGGATGGGTGGTATGTAGGG	0	-	9	65017745	674
unique	G4_F	CAACAAGTGGGCTCTCATCG	0	+	8	2929359	G4_R	AGGCCATGTTCTGTTCTCGA	0	-	8	2930140	782
multi2	G5_R	AGACACATTCGCCGTTTCTG	0	+	6	120703278	G5_F	ACAGCGTCACCAACTACATG	0	-	6	120703621	344
multi2	G5_R	AGACACATTCGCCGTTTCTG	0	+	6	120724131	G5_F	ACAGCGTCACCAACTACATG	0	-	6	120724474	344
```
Note that unique indicates the primer pair can uniquely amplify a target; multi2 indicates that two reigons are targeted.

