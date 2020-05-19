# A set of memory economical functions for fast parallel assembly of genome mapped RNA-seq reads

Well, say we have to cope with a very large genome, human for example. Say we have a set of BAM files holding alignments of stranded RNA-seq reads to the genome sequence. We want to dig out transcriptomic data, that is the **sequence** of the transcripts which are actually present in the sample tissue/organ/organism of interest.

Usually, alignments in BAM files are sorted either by reference (i.e. pseudo-chromosome/scaffold/contig) ID, then coordinate, or by read name. For the purpose of grouping together all alignments that map "overlappingly" to the genome sequence, we have first to perform a kind of **hybrid sorting**: alignments are sorted by reference id / coordinate only if their come single-end read or first member of paired-ends reads. The second member of paired-ends reads, that map farther than the first one, is placed - in that hybrid sorting method - immediately next to its kin.

Blocks of alignments - from reads that originate from the **same strand** - are then all aligned together, simply considering the genomic coordinates. Then, a consensus sequence and associated CIGAR line are computed. If the mapping (stranded) region overlaps with some known genomic features, their ID is also provided. Yeahh, parsing all that data in clean gff3 compliant files, we can visualize it with a cool genome browser, like JBrowse.

Presently, only the consensus sequence is extracted, choosing at every genomic position the most common nucleotide. I may consider in some future to build every possible transcript, using a graph-based method. I'm still not sure that it would really be useful..

Now, technical specifications:
* python and python packages version:
  * python 3.6.7
  * pysam 0.15.2
  * HTSeq 0.11.2
* hardware:
  * 12 cpu Intel(R) Xeon(R) CPU E5-2609 v3 @ 1.90GHz
  * 65892288 kB RAM
* datasets: twelve holding respectively 44, 52, 36, 39, 38, 29, 40, 43, 35, 27, 27, 28 millions alignments
with mixed single-end / paired-ends reads in averaged proportion 5 / 2.

Enjoy trying to run this code and please don't neglect to share experience feedback !
