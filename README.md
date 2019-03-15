# tRNA gene classifier

This program uses DNA data alone to predict tRNA gene expression, using binary (active/inactive) classifications. The premise of this project was based heavily on the observations made in this study: https://www.pnas.org/content/pnas/early/2018/08/17/1801240115.full.pdf. A separate manuscript focused on the development of this classifier will be posted on bioRxiv very soon.

This program was built with a focus on going from a HAL object to tRNA classifications. However, many labs use MAFs instead of HALs, or may have already reduced their HALs to other forms. To handle this, the pipeline has many files to be used in the order given, but any step can be skipped if you already have the file that that step produces.

By the time you are ready to run the script that makes the actual classifications, you should have:
- .wig file with PhyloP scores for all bases spanning from 20 bases upstream to 10 bases downstream of each tRNA gene
- .bed file showing the coordinates of each tRNA gene (from tRNAscan-SE)
- .out file containing the bit-scores of each tRNA gene (from tRNAscan-SE)
- .fa file with DNA sequence spanning from 250 bases upstream to 250 bases downstream of each tRNA gene (or to the end of the chromosome/scaffold)
- RNAfold output showing the minimum free energy for each tRNA gene
- .bed file containing the locations of annotated protein coding genes in your genome of interest

To run from start to finish, the only dependencies are tRNAscan-SE, HAL and PHAST, and PHAST is itself a dependency of HAL. These instructions for HAL are thorough and should work for most systems: https://github.com/ComparativeGenomicsToolkit/hal

You can download tRNAscan-SE at http://lowelab.ucsc.edu/tRNAscan-SE/. You can also download the necessary data directly from http://gtrnadb.ucsc.edu.

### Graphical Overview:

<img src='classifierPipelineNew.png' alt='classifier pipeline' width='680'/>

Here is a general guide to the program in the listed order. All commands ending in .py are custom programs that can be found in this repository. The rest are either functions of HAL, PHAST or tRNAscan-SE:

`hal2fasta /path/to/hal-file species-name > genome.fa # 1
tRNAscan-SE genome.fa -o tRNA.out -f tRNA.ss -s tRNA.iso -m tRNA.stats -b tRNA.bed -a tRNA.fa -H -y --detail # 2
EukHighConfidenceFilter -r -i tRNA.out -s tRNA.ss -p tRNA_hiConf -o /path/to/desired/output/ # 2
python makeHiConfBed.py tRNA_hiConf.out tRNA.bed # 3`


