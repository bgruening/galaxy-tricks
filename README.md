# Useful tricks and tipps for Galaxy users

"You must feel the Force around you." *Yoda*

- [Text processing](#text-processing)
- [HTS](#hts)
- [Workflows](#workflows)
- [More resources](#more-resources)
- [Disclaimer](#disclaimer)


## Text processing
- Convert comma separated files into tab-separated files<br>
   `Convert delimiters to TAB`
- FASTA files with unique sequences<br>
   `FASTA-to-Tabular` → `Unique occurrences of each record` (advanced parameters) → `Tabular-to-FASTA`
- Remove sequences with `N` or any other character<br>
  `FASTA-to-Tabular` → `Filter data on any column using simple expressions` with <br>(condition: `c2.find('N') != -1`) → `Tabular-to-FASTA`
- Extracting the 3rd column from a 5 column file<br>
  `Cut columns from a table` with `c3`
- Reorder columns or column swap<br>
  `Cut columns from a table` with `c3,c2,c1` 
- Count how often one entry appears in column 1<br>
  `Datamash` with `Group by fields`: 1 and `Operation to perform`: count
- Group all rows where column 1, 4 and 5 are identical<br>
  `Datamash` with `Group by fields`: 1,4,5
- Column-to-rows and rows-to-columns (transpose matrix)<br>
  `Transpose rows/columns`
- Make your files smaller, e.g. for testing; subsampling of files<br>
  `Select random lines from a file`
- Make your sequence files smaller, e.g. for testing; subsampling sequences<br>
  `Sub-sample sequences files`
- Merge two files together according to one column in every file<br>
  `Join two files`
- Add unique column<br>
  `Add column to an existing dataset` with `iterate`: Yes
- Get rid of all rows where column 2 has values greater than 0<br>
  `Filter data on any column using simple expressions` with `c2>0`
- Get all rows where column 4 starts with hsa<br>
  `Filter data on any column using simple expressions` with `c4.startswith('hsa')`
- Get rid of all rows where the sum of column 2 and 3 is greater than 10<br>
  `Filter data on any column using simple expressions` with `c2+c3>10`
- Get rid of all rows where the length of my text in column 2 is greater than 10<br>
  `Filter data on any column using simple expressions` with `len(c2)>10`
- Create new rows for every comma separated value in column 3; Unfolding<br>
  `Unfold columns from a table` with `Column 3` and `Comma`
- Split the first four characters of a line into it's own column<br>
  `Replace Text in entire line` with `Find Pattern`: ^(.{4}) and `Replace Pattern`: &\t
- Add the basepairs "TA" to the end of each sequences<br>
  `FASTA to Tabular` → `Add column` with `TA` → `Merge Columns` → `Cut columns` → `Tabular to FASTA`

## HTS
- Map RNA-seq data<br>
  `HISAT` or `TopHat`
- Map DNA-seq data<br>
  `Bowtie` or `BWA`
- Map methylC-seq data<br>
  `Bismark`
- Get all genes that are covert by reads<br>
  `htseq-count` with a gene annotation [GTF file](http://www.ensembl.org/info/website/upload/gff.html) on your BAM file  → `Filter data on any column using simple expressions` with `c2>0`
- Extract sequences from intercal files, like gff, bed, gtf. Returning FASTA file →<br>
  `Extract Genomic DNA using coordinates from assembled/unassembled genomes`

## Workflows
- Find two genes located nearby<br>
  [Description](https://github.com/bgruening/galaxytools/tree/master/workflows/ncbi_blast_plus/find_genes_located_nearby) :: [Tool Shed](https://toolshed.g2.bx.psu.edu/view/bgruening/find_genes_located_nearby_workflow)


## More Resources
 - Galaxy 101 - a must read for all HTS Padawan: https://wiki.galaxyproject.org/Learn/GalaxyNGS110
 - A lot of videos to learn using Galaxy while eating popcorn: https://vimeo.com/galaxyproject

## Disclaimer
All tools mentioned here are available from the Galaxy Tool Shed. Kindly ask your Galaxy Administrator to get access to them.
