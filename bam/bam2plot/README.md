### bam2plot
The script uses BAM files alongside their indexed files as input data and plot read distribution of each BAM data on a specified region. 

```
source("bam2regionPlot.R")
bam2regionPlot(bamlist.file = "bam.list.txt", check.bamlist = F,
               labels = c("R3CK", "R3CMN", "R4CK", "R4CMN", "S1CK", "S1CMN", "S4CK", "S4CMN"),
               chr = "chr1", from = 196215972, to = 196221349, rc = "rpm",
               cex.axis = 0.6, cex.main = 1, cex.lab = 0.9)     
```