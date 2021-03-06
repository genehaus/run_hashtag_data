# Run_hashtag_data


	This information was written based on my experiences 
	and for this, I referred to others' work and lots of webiste.   


1. Install CITE-seq-Count<br>
https://github.com/Hoohm/CITE-seq-Count

```
pip install CITE-seq-Count --user 
```


2. Pre-required data

	1. tags.csv<br>
	2. whitelist = barcodes.tsv

		Download the file from https://github.com/10XGenomics/cellranger/blob/master/lib/python/cellranger/barcodes/translation/3M-february-2018.txt.gz <br>
		(This file is same as the barcodes.tsv from raw_feature_bc_matrix)	<br>
		or get barcodes.tsv from "filtered_feature_bc_matrix", one of the output from CellRanger 


3. Run 

with Python 3

```
#!/usr/XXX/bin/zsh
#SBATCH -J CITE-seq-Count
#SBATCH -t 100:00:00
#SBATCH --mem-per-cpu=10240M
#SBATCH --output=output.%J.txt
/XXX/CITE-seq-Count -R1 /XXX/XXX_S1_L001_R1_001.fastq.gz,/XXX/XXX_S1_L002_R1_001.fastq.gz -R2 /XXX/XXX_S1_L001_R2_001.fastq.gz,/XXX/XXX_S1_L002_R2_001.fastq.gz -t /XXX/tags.csv -cbf 1 -cbl 16 -umif 17 -umil 28 -cells 0 -wl /XXX/barcodes.tsv -o /XXX/XXX_ouput/
```

XXX should be full directory or file name 

4. Issue

```
Date: 2019-12-06
Running time: 1.0 hour, 8.0 minutes, 9.714 seconds
CITE-seq-Count Version: 1.4.3
Reads processed: 25334524
Percentage mapped: 0
Percentage unmapped: 100
Uncorrected cells: 0
Correction:
        Cell barcodes collapsing threshold: 1
        Cell barcodes corrected: 1391
        UMI collapsing threshold: 2
        UMIs corrected: 148
Run parameters:
        Read1_paths: /XXX/XXX_S1_L001_R1_001.fastq.gz
	...
        Read2_paths: /XXX/XXX_S1_L001_R2_001.fastq.gz
	...
        Cell barcode:
                First position: 1
                Last position: 16
        UMI barcode:
                First position: 17
                Last position: 28
        Expected cells: 0
        Tags max errors: 2
        Start trim: 0
```


5-1. How to solve the issue I 
 
I checked the unmapped.csv if antibody sequence exists or not, but it wasn't.<br> 
I think CITE-seq-Count doesn't have issue.<br>
So, I plan to compare the number of read which have antibody sequence with the output of seurat by taking as an input, 3 files from "umi_count" 
```
zcat *.fastq.gz | grep --color=always antibody_sequence   
```


5-2. How to solve the issue II

I downloaded the hashtag data (HTO) from https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE108313 <br>
and got the hashtag antibody sequences from https://www.biorxiv.org/content/10.1101/237693v1.full.pdf (sample pooling) <br>
and then run CITE-seq-Count again <br>


In script 
```
/XXX/XXX/.pyenv/shims/CITE-seq-Count -R1 /XXX/XXX/Seurat_hashtag_HTO/SRR8281307_1.fastq.gz -R2 /XXX/XXX/Seurat_hashtag_HTO/SRR8281307_2.fastq.gz -t /XXX/XXX/Seurat_hashtag_HTO/tags.csv -cbf 1 -cbl 16 -umif 17 -umil 29 -cells 0 -wl /XXX/XXX/Seurat_hashtag_HTO/barcodes.tsv -o /XXX/XXX/Seurat_hashtag_HTO/
```

In output


```
Date: 2020-01-08
Running time: 3.0 hours, 6.0 minutes, 37.79 seconds
CITE-seq-Count Version: 1.4.3
Reads processed: 74219921
Percentage mapped: 98
Percentage unmapped: 2
Uncorrected cells: 0
Correction:
        Cell barcodes collapsing threshold: 1
        Cell barcodes corrected: 51820
        UMI collapsing threshold: 2
        UMIs corrected: 113944
Run parameters:
        Read1_paths: /XXX/XXX/XXX/Seurat_hashtag_HTO/SRR8281307_1.fastq.gz
        Read2_paths: /XXX/XXX/XXX/Seurat_hashtag_HTO/SRR8281307_2.fastq.gz
        Cell barcode:
                First position: 1
                Last position: 16
        UMI barcode:
                First position: 17
                Last position: 29
        Expected cells: 0
        Tags max errors: 2
        Start trim: 0
```



6. Conclusion 

CITE-seq count has not issue<br> 
My fastq file has the issue<br>



**References & Good Q&A web source**

https://github.com/Hoohm/CITE-seq-Count<br>
https://github.com/Hoohm/CITE-seq-Count/issues/82<br>
https://github.com/Hoohm/CITE-seq-Count/issues/62<br>



