transcriptome-and-genome-assembly
=================================
Scripts in this repository have been used to assemble de novo transcriptomes and genomes 

prinseq.sh - paired end read cleaning with prinseq. If your reads are not illumina cassava (post v1.8) you should comment out lines 28 and 29. This script uses a script called stats.sh to monitor and report resource usage on a SGE cluster like Beocat at KSU. But this section (lines 36-42) also can be commented out.  

stats.sh - see prinseq.sh
 
 
mira_454.sh - Runs Mira 3.4 for 454 data alone. If reads have been cleaned than the trace .xml file extracted from the .sff file will be missing so the parameters in this script are set with mxti=no to reflect the lack of xml trace files.

velvet_oases_k-mer_assemblies.sh - This script will write single k-mer transcriptome assembly scripts and a list of qsubs that can be used to call them on beocat. Comment out line 10 if your sequences are already shuffled. After read cleaning I load my broken pairs (now single end reads) as "-short" and pairs as "-shortPaired". The script calculates memory request for each assembly as "Ram required for velvetg = -109635 + 18977*ReadSize + 86326*GenomeSize + 233353*NumReads - 51092*K" which is taken from http://listserver.ebi.ac.uk/pipermail/velvet-users/2009-June/000359.html. You should change the ReadSize, GenomeSize, and NumReads to reflect your data.

oases_merge.sh - This script calls velvet and oases to merge many single k-mer assemblies.

run_Trinity_Icyan_ALL.sh - This script calls Trinity to assemble with a single k-mer. After read cleaning broken pairs can be loaded as (now single end reads) as "--single".

master_ABySS.pl - this scripts call ABySS_script_writer.pl to write single k-mer genome assembly scripts then executes them (on an SGE cluster). Comment out line 15 and uncomment line 16 to run assemblies every odd k-mer between k=51 and k=71 (the range often seen to have the highest N50).

ABySS_script_writer.pl - see master_ABySS.pl

