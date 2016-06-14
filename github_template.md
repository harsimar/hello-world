##Ensembl Gene Definitions
All annotated protein coding genes in the ensembl human gene set. 

|Downloads| Database Definition | Notes |
|---------|--------------|------------|-----|
|[hg19](http://ftp.ensembl.org/pub/release-74/gtf/homo_sapiens/Homo_sapiens.GRCh37.74.gtf.gz)<br>[Recent release](http://ftp.ensembl.org/pub/current_gtf/homo_sapiens/Homo_sapiens.GRCh38.78.gtf.gz)| [Database Definition](https://github.com/ITMI/p7_pilot/blob/master/annotation/Ensembl/create_ensembl_gene.sql) | [Documentation](http://ftp.ensembl.org/pub/release-74/gtf/homo_sapiens/README)<br>Recent release links to most recent human reference build. <br> The ensembl_distinct table provides a smaller, distinct subset of ensembl gene/transcript/exon annotations| 

## Sample Query

The following query can be used to annotate a subset of variants with ensembl gene, transcript and exon information. 


```sql
--SELECT subset of illumina variants to annotate 
with vars AS 
   (SELECT subject_id,
         chrom,
         pos,
         ref,
         allele,
         gt FROM wgs_ilmn.vcf_variant
  -- limit subset to only variants in chromosome 1 
  -- remove to return ALL chromosomes 
   WHERE chrom = '1' ) 
   
  -- join variant subset with ensembl_gene table
   SELECT vars.*,
         ens.gene_name,
         ens.gene_id,
         ens.transcript_name,
         ens.transcript_id,
         ens.exon_id,
         ens.exon_number FROM anno_grch37.ensembl_gene AS ens, vars 
         WHERE vars.chrom = ens.chrom 
         AND vars.pos between ens.pos and ens.stop 
```