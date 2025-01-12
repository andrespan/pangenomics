---
title: "Interactive pangenome graphics"
teaching: 15
exercises: 40
questions:
- "How can I obtain an interactive pangenome plot?"
- "How can I measure the homogeneity of the gene families?"
- "How to obtain an enrichement analysis of the gene families?"
- "How to compute the ANI values between the genomes of the pangenome?"

objectives:
- "Construct a pangenome following the Anvi'o workflow"
- "Visualize and interact with the pangenome graph"
- "Compute and visualize the ANI values of the genomes from the pangenome"
- "Perform a functional enrichment analysis on a group of genomes from the pangenome"

keypoints:
- "Anvi’o can build a pangenome starting from genomes or metagenomes, or a combination of both"
- "Anvi'o allows to interactively visualize your pangenomes"
- "Anvi'o platform includes additional scripts to explore the geometric and biochemical homogeneity of the gene clusters, to compute and visualize the ANI values of the genomes, to conduct a functional enrichment analysis in a group of genomes, among others"
---

<a href="../fig/01-04-01.png">
  <img src="../fig/01-03-01.png" alt="Anvi'o network representation" />
</a>

## Anvi'o

Anvi’o is an open-source, community-driven analysis and visualization platform for microbial ‘omics.
It brings together many aspects of today's cutting-edge strategies including **genomics, metagenomics, metatranscriptomics, phylogenomics, microbial population genetis, pangenomics and metapangenomis** in an *integrated* and *easy-to-use* fashion thorugh extensive interactive visualization capabilities. 



## Get all ready to start the Anvi'o workflow to build a pangenome

To start using Anvi'o, activate the conda environment **Pangenomics** 

~~~
conda activate Pangenomics
~~~
{: .source}

~~~
(Pangenomics) betterlab@betterlabub:~/Pangenomics/Anvio/MTBC$
~~~
{: .output}

Move into the directory named **results** and create a new directory called **anvi-o** for the Anvi'o analysis
~~~
cd dc-workshop/results
mkdir anvi-o
cd anvi-o
~~~
{: .source}

In order to better organize our Anvi'o results, create a new directory named **genome-db** that will be used to storage the genome database needed for the Anvi'o pangenome worflow
~~~
mkdir genome-db
~~~
{: .source}

**Note:** The bacterial genomes that will be used in this practice come from the Prokka annotation analysis. We will use the .gbk files as input for the Anvi'o workflow. The .gbk files can be found in ~/dc-workshop/results/annotated


Let's do it!


Ten steps guide to build a Pangenome in Anvi'o
===============================================

Step 1
===============================================

Process the genome files (.gbk) with the `anvi-script-process-genbank` script

~~~
ls ~/dc-workshop/results/annotated/agalactiae* | cut -d'/' -f7 | cut -d '.' -f1 | while read line; do anvi-script-process-genbank -i GENBANK --input-genbank ~/dc_workshop/results/annotated/$line.gbk -O genome-db/$line; done
~~~
{: .source}

~~~
cd genome-db
ls
~~~
{: .source}

~~~
agalactiae_18RS21-contigs.fa               agalactiae_A909-contigs.fa                 agalactiae_COH1-contigs.fa
agalactiae_18RS21-external-functions.txt   agalactiae_A909-external-functions.txt     agalactiae_COH1-external-functions.txt
agalactiae_18RS21-external-gene-calls.txt  agalactiae_A909-external-gene-calls.txt    agalactiae_COH1-external-gene-calls.txt
agalactiae_515-contigs.fa                  agalactiae_CJB111-contigs.fa               agalactiae_H36B-contigs.fa
agalactiae_515-external-functions.txt      agalactiae_CJB111-external-functions.txt   agalactiae_H36B-external-functions.txt
agalactiae_515-external-gene-calls.txt     agalactiae_CJB111-external-gene-calls.txt  agalactiae_H36B-external-gene-calls.txt
~~~
{: .output}

Step 2
===============================================
Reformat the fasta files using the `anvi-script-reformat-fasta` script

~~~
ls *fa |while read line; do anvi-script-reformat-fasta --seq-type NT $line -o $line\.fasta; done
~~~
{: .source}

~~~
agalactiae_18RS21-contigs.fa               agalactiae_A909-contigs.fa                 agalactiae_COH1-contigs.fa
agalactiae_18RS21-contigs.fa.fasta         agalactiae_A909-contigs.fa.fasta           agalactiae_COH1-contigs.fa.fasta
agalactiae_18RS21-external-functions.txt   agalactiae_A909-external-functions.txt     agalactiae_COH1-external-functions.txt
agalactiae_18RS21-external-gene-calls.txt  agalactiae_A909-external-gene-calls.txt    agalactiae_COH1-external-gene-calls.txt
agalactiae_515-contigs.fa                  agalactiae_CJB111-contigs.fa               agalactiae_H36B-contigs.fa
agalactiae_515-contigs.fa.fasta            agalactiae_CJB111-contigs.fa.fasta         agalactiae_H36B-contigs.fa.fasta
agalactiae_515-external-functions.txt      agalactiae_CJB111-external-functions.txt   agalactiae_H36B-external-functions.txt
agalactiae_515-external-gene-calls.txt     agalactiae_CJB111-external-gene-calls.txt  agalactiae_H36B-external-gene-calls.txt
~~~
{: .output}

Step 3
===============================================
Create a database per genome with the `anvi-gen-contigs-database` script

~~~
ls *fasta | while read line; do anvi-gen-contigs-database -T 4 -f $line -o $line-contigs.db; done
ls
~~~
{: .source}

~~~
agalactiae_18RS21-contigs.fa                   agalactiae_A909-contigs.fa                     agalactiae_COH1-contigs.fa
agalactiae_18RS21-contigs.fa.fasta             agalactiae_A909-contigs.fa.fasta               agalactiae_COH1-contigs.fa.fasta
agalactiae_18RS21-contigs.fa.fasta-contigs.db  agalactiae_A909-contigs.fa.fasta-contigs.db    agalactiae_COH1-contigs.fa.fasta-contigs.db
agalactiae_18RS21-external-functions.txt       agalactiae_A909-external-functions.txt         agalactiae_COH1-external-functions.txt
agalactiae_18RS21-external-gene-calls.txt      agalactiae_A909-external-gene-calls.txt        agalactiae_COH1-external-gene-calls.txt
agalactiae_515-contigs.fa                      agalactiae_CJB111-contigs.fa                   agalactiae_H36B-contigs.fa
agalactiae_515-contigs.fa.fasta                agalactiae_CJB111-contigs.fa.fasta             agalactiae_H36B-contigs.fa.fasta
agalactiae_515-contigs.fa.fasta-contigs.db     agalactiae_CJB111-contigs.fa.fasta-contigs.db  agalactiae_H36B-contigs.fa.fasta-contigs.db
agalactiae_515-external-functions.txt          agalactiae_CJB111-external-functions.txt       agalactiae_H36B-external-functions.txt
agalactiae_515-external-gene-calls.txt         agalactiae_CJB111-external-gene-calls.txt      agalactiae_H36B-external-gene-calls.txt
~~~
{: .output}

Step 4
===============================================
When using external genomes in anvi'o, a list of the genome ids and their corresponding genome database is required. This list tells Anvi'o which genomes will be processed to construct the pangenome. 
~~~
ls *.fa | cut -d '-' -f1 | while read line; do echo $line$'\t'$line-contigs.db >>external-genomes.txt; done
head external-genomes.txt
~~~
{: .source}

~~~
agalactiae_18RS21	agalactiae_18RS21-contigs.db
agalactiae_515	agalactiae_515-contigs.db
agalactiae_A909	agalactiae_A909-contigs.db
agalactiae_CJB111	agalactiae_CJB111-contigs.db
agalactiae_COH1	agalactiae_COH1-contigs.db
agalactiae_H36B	agalactiae_H36B-contigs.db
~~~
{: .output}

Step 5
===============================================
Modify the headers of the list external-genomes.txt
~~~
nano external-genomes.txt
~~~
{: .source}

~~~
  GNU nano 4.8                                                             external-genomes.txt                                                                       
agalactiae_18RS21       agalactiae_18RS21-contigs.db
agalactiae_515  agalactiae_515-contigs.db
agalactiae_A909 agalactiae_A909-contigs.db
agalactiae_CJB111       agalactiae_CJB111-contigs.db
agalactiae_COH1 agalactiae_COH1-contigs.db
agalactiae_H36B agalactiae_H36B-contigs.db


^G Get Help     ^O Write Out    ^W Where Is     ^K Cut Text     ^J Justify      ^C Cur Pos      M-U Undo        M-A Mark Text   M-] To Bracket  M-Q Previous
^X Exit         ^R Read File    ^\ Replace      ^U Paste Text   ^T To Spell     ^_ Go To Line   M-E Redo        M-6 Copy Text   ^Q Where Was    M-W Next
~~~
{: .output}

~~~
head external-genomes.txt
~~~
{: .source}

~~~
name	contigs_db_path
agalactiae_18RS21	agalactiae_18RS21-contigs.db
agalactiae_515	agalactiae_515-contigs.db
agalactiae_A909	agalactiae_A909-contigs.db
agalactiae_CJB111	agalactiae_CJB111-contigs.db
agalactiae_COH1	agalactiae_COH1-contigs.db
agalactiae_H36B	agalactiae_H36B-contigs.db
~~~
{: .output}

Step 6
===============================================
Rename the .db files

~~~
rename s'/.fa.fasta-contigs.db/.db/' *db
ls *.db
~~~
{: .source}

~~~
agalactiae_18RS21-contigs.db  agalactiae_A909-contigs.db    agalactiae_COH1-contigs.db
agalactiae_515-contigs.db     agalactiae_CJB111-contigs.db  agalactiae_H36B-contigs.db
~~~
{: .output}

Step 7
===============================================
Execute HMM analysis with the `anvi-run-hmms` script to identify matching genes in each contigs database file

~~~
ls *contigs.db | while read line; do anvi-run-hmms -c $line; done
ls
~~~
{: .source}

~~~
Salida
~~~
{: .output}


Step 8
===============================================
Create the genome database `genomes-storage-db` using the `anvi-gen-genomes-storage` script. In this case, we named this `genomes-storage-db` as **AGALACTIAE_GENOMES.db**, which will be used downstream as input in other process.

~~~
anvi-gen-genomes-storage -e external-genomes.txt -o AGALACTIAE_GENOMES.db
ls
~~~
{: .source}

~~~
AGALACTIAE_GENOMES.db                      agalactiae_A909-contigs.db                 agalactiae_COH1-contigs.fa
agalactiae_18RS21-contigs.db               agalactiae_A909-contigs.fa                 agalactiae_COH1-contigs.fa.fasta
agalactiae_18RS21-contigs.fa               agalactiae_A909-contigs.fa.fasta           agalactiae_COH1-external-functions.txt
agalactiae_18RS21-contigs.fa.fasta         agalactiae_A909-external-functions.txt     agalactiae_COH1-external-gene-calls.txt
agalactiae_18RS21-external-functions.txt   agalactiae_A909-external-gene-calls.txt    agalactiae_H36B-contigs.db
agalactiae_18RS21-external-gene-calls.txt  agalactiae_CJB111-contigs.db               agalactiae_H36B-contigs.fa
agalactiae_515-contigs.db                  agalactiae_CJB111-contigs.fa               agalactiae_H36B-contigs.fa.fasta
agalactiae_515-contigs.fa                  agalactiae_CJB111-contigs.fa.fasta         agalactiae_H36B-external-functions.txt
agalactiae_515-contigs.fa.fasta            agalactiae_CJB111-external-functions.txt   agalactiae_H36B-external-gene-calls.txt
agalactiae_515-external-functions.txt      agalactiae_CJB111-external-gene-calls.txt  external-genomes.txt
agalactiae_515-external-gene-calls.txt     agalactiae_COH1-contigs.db
~~~
{: .output}

Step 9
===============================================
Construct the pangenome database `pan-db` with the `anvi-pan-pangenome` script using the `genomes-storage-db` named **AGALACTIAE_GENOMES.db** as input 

~~~
anvi-pan-genome -g AGALACTIAE_GENOMES.db \
                --project-name "PANGENOME-AGALACTIAE" \
                --output-dir AGALACTIAE \
                --num-threads 6 \
                --minbit 0.5 \
                --mcl-inflation 10 \
                --use-ncbi-blast
~~~
{: .source}

~~~
SALIDA
~~~
{: .output}


Step 10
===============================================
Create the interactive pangenome with the `anvi-display-pan` script using as input the `genomes-storage-db` **AGALACTIAE_GENOMES.db** and the `pan-db` **PANGENOME-AGALACTIAE-PAN.db** (located in AGALACTIAE directory)

~~~
anvi-display-pan -g AGALACTIAE_GENOMES.db \
    -p AGALACTIAE/PANGENOME-AGALACTIAE-PAN.db
~~~
{: .source}

~~~
SALIDA
~~~
{: .output}


Whitout disturbing the active terminal, open a new window in your prefered browser (recommended Chrome), copy-paste the following link and click on the bottom `Draw` to see your results and start interacting with your pangenome

~~~
http://132.248.196.38:8080
~~~
{: .output}

  
> ## Exercise 1: The homogeneity of gene clusters.
>Anvi’oo allows us to identify different levels of disagreements between amino acid sequences in different genomes. Amino acid sequences from different genomes in a gene cluster that are almost identical tell us that the gene cluster is highly homogeneous. 
>
> The **geometric homogeneity index** tell us the degree of geometric configuration between the genes of a gene cluster and the **functional homogeneity index** considers aligned residues and quantifies differences across residues in a site.
>
>For more info see [this.](https://merenlab.org/2016/11/08/pangenomics-v2/#inferring-the-homogeneity-of-gene-clusters)
>
>Go to this [page](https://anvio.org/help/main/programs/anvi-get-sequences-for-gene-clusters/) and explore the pangenome graph according to the following homogeneity index.
>
>a) Order the pangenome based on the geometric homogeneity index and inspect a gene cluster with a relatively low score.
>
>b) Filter the gene cluster according to a functional homogeneity index above 0.25. 
>
>Extra) How can you estimate evolutionary relationships between genomes? With the `concatenated-gene-alignment-fasta` produce the phylogenomic tree and explore it.
> >## Solution
>> a) Go to the main settings panel and modify the “items order”.
>>
>> b) 
>>~~~
anvi-get-sequences-for-gene-clusters -g genomes-storage-db \
                                     -p pan-db \
                                     -o genes-fasta \
                                     --min-functional-homogenity-index 0.25
>>~~~
>>{: .language-bash}
>>
>>Extra)Explore this [page.](https://anvio.org/help/main/programs/anvi-gen-phylogenomic-tree/)
>{: .solution}
{: .challenge}
  
  
> ## Exercise 2: Splitting the pangenome.
> 1. Read about [anvi-split](https://anvio.org/help/main/programs/anvi-split/) 
> 2. With this program split your pangenome in independent pangenomes that:
> > * Contains only singletons.
> > * Contains only core gene clusters.
>
> Tip: [anvi-display-pan](https://anvio.org/help/main/programs/anvi-display-pan/) can be usefull
{: .challenge}

{% include links.md %}
