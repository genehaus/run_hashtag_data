# Run_hashtag_data


	This information was written based on my experiences<br> 
	and I referred to others' work and webiste.   


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

```
#!/usr/XXX/bin/zsh
#SBATCH -J CITE-seq-Count
#SBATCH -t 100:00:00
#SBATCH --mem-per-cpu=10240M
#SBATCH --output=output.%J.txt
/XXX/CITE-seq-Count -R1 /XXX/XXX_S1_L001_R1_001.fastq.gz,/XXX/XXX_S1_L002_R1_001.fastq.gz -R2 /XXX/XXX_S1_L001_R2_001.fastq.gz,/XXX/XXX_S1_L002_R2_001.fastq.gz -t /XXX/tags.csv -cbf 1 -cbl 16 -umif 17 -umil 28 -cells 0 -wl /XXX/barcodes.tsv -o /XXX/XXX_ouput/
```

XXX should be full directory or file name 

4. Issues 

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

So, I checked the unmapped.csv if antibody sequence exists or not, but it wasn't.<br> 
So, I plan to compare the number of read which have antibody sequence with the output of seurat by taking as an input, 3 files from "umi_count" 
```
zcat *.fastq.gz | grep --color=always antibody_sequence   
```

**References & Good Q&A web source**

https://github.com/Hoohm/CITE-seq-Count<br>
https://github.com/Hoohm/CITE-seq-Count/issues/82<br>
https://github.com/Hoohm/CITE-seq-Count/issues/62<br>



