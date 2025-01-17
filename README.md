# Hackbio_Transcriptomics
Welcome to the Hackbio Transcriptomics biostack repository



RNA-Seq Analysis Using Galaxy

In this tutorial we identified genes and pathways regulated by the Pasilla gene (the Drosophila homologue of the mammalian splicing regulators Nova-1 and Nova-2 proteins) using RNA-Seq data generated by Brooks et al. 2011.

We compared the genes expression in; 

4 untreated samples: GSM461176, GSM461177, GSM461178, GSM461182

3 treated samples (Pasilla gene depleted by RNAi): GSM461179, GSM461180, GSM461181



Method

## 1. Dataset upload into Galaxy

The FastQ files of the dataset were downloaded from [here](https://zenodo.org/record/4541751)

## 2. Quality Control : FastQC and Cutadapt
    FastQC
 FastQC report for sample [GSM461177](https://github.com/S-m-Baffoe/Hackbio_Transcriptomics/blob/main/GSM461177.pdf) and [GSM461180](https://github.com/S-m-Baffoe/Hackbio_Transcriptomics/blob/main/GSM461180.pdf)  

    Trimming:
For sample GSM461177, 1.8% of bp was trimmed

![GSM461177 Cutadapt](https://user-images.githubusercontent.com/68198076/139517060-eb13c924-e5e0-41f9-ab14-1e73d7340971.PNG)

For sample GSM461180, 6.8% of bp was trimmed

![GSM461180 Cutadapt](https://user-images.githubusercontent.com/68198076/139517153-6a2b55ee-1f36-453b-ad77-bee832eb07aa.PNG)

## 3. Mapping: RNA STAR visualized using IGV
We have used the trimmed or quality controlled sequences for mapping, it was necessary to know from where the sequence originated from in the genome. Since in this tutorial authors used the Drosophila melanogaster cells, we used the reference genome of Drosophila melanogaster dm6.
Now in eukaryotic genomes, transcriptomes arise from processed mRNA, i.e Spliced. So to map efficiently we imported the spliced annotation of D. melanogaster and used RNA STAR for mapping.
### -> RNA-STAR
![rna star](https://user-images.githubusercontent.com/92271396/139474763-e48f8ee4-3fd6-40f0-a329-35fe22497401.png)

This was the Alignment scores that were obtained

![data](https://user-images.githubusercontent.com/92271396/139475289-78246e72-f581-4fb4-9424-0fbc3ba647e7.png)

For GSM461177, 83.1% reads are mapped exactly once

### -> Inspection of mapping results
#### i) Visualization using local Integrative Genomics Viewer (IGV) 
![igv](https://user-images.githubusercontent.com/92271396/139480360-d11dfd0b-b8dd-4678-9189-7705781f92e4.png)
 The connecting lines between some of the alignments represents the reads that were mapped against introns i.e Splice sites.
 
 We then plotted the Sashimi Plot that gave us a better visualisation for the position of spliced junctions(RED ARCS)
 ![sashimi](https://user-images.githubusercontent.com/92271396/139480485-46d55743-d7ab-4fe6-bda8-b91b5f87cc22.png)
 
 Further steps involved are for the more refined inspection of Mapping

#### ii) Checking Duplicate reads using Mark Duplicates
FastQC report revealed some duplicate reads, to assess the percentage of those we carried out Mark duplicates
![stats](https://user-images.githubusercontent.com/92271396/139480851-f5efdf78-e1e0-4d18-a038-693f6a042c93.png)

The percentage of duplicates were acceptable for both our samples.

#### iii) Number of reads mapped to each chromosome using Samtools idxstats

To assess the quality of samples, i.e whether some genes are highly expressed over the other, we did SamTools Idxstats, to know the number of reads mapped to each chromosomes.

![stats 2](https://user-images.githubusercontent.com/92271396/139481202-98c5d912-6d9f-4384-998e-119c1d29ded3.jpg)

The reads mapped mostly to chromosome 2 (chr2L and chr2R), 3 (chr3L and chr3R) and X.

![1](https://user-images.githubusercontent.com/92271396/139481270-fe861e02-2748-4ced-b3eb-40b84ecffc3b.png)

Most of the reads map to X(97.8%) and only a few to Y(2.2%) indicating there are probably not many genes on Y, so the samples are probably both female.

#### iv) Gene Body Coverage
And then we checked the Gene Body Coverage, to evaluate the uniformity and check for 5’/3’ bias.

![2](https://user-images.githubusercontent.com/92271396/139481521-89a61c93-6c9c-4ffe-8c17-f564ca528e5a.png)

The analysis gave the results, that the gene body coverage is quite even, as evident from the plot.

#### v) Read distribution across features or regions
The final step in inspection of Mapping results was to check the distribution of genes in exons, 3’ Untranslated regions(UTR) , 5’UTR, and introns, since its a RNA-seq analysis, mostly reads should map to exons and not introns.

![3](https://user-images.githubusercontent.com/92271396/139481801-349b10b8-88ca-414a-b396-4fc15a1b25c7.jpg)

For one of the sample the percentage of reads mapped to exons is 80% and that to introns is 2.4%

![4](https://user-images.githubusercontent.com/92271396/139481845-0f4c9a1d-d050-4ca4-8608-026169eb70d6.png)

For another sample the percentage of reads mapped to exons is 81.1% and that to introns is 2%

![5](https://user-images.githubusercontent.com/92271396/139481894-5ac47b5e-4885-4037-bd11-b4675855fb29.png)

These results confirm that our sequences were RNA-Seq data, and those were mapped successfully.

## 4. Quantification: featureCounts
The next step is to quantify the number of reads mapped to the exon of the gene

Libraray Strandness was determined by Infer Experiment galaxy tool
For GSM461177 – Untreated sample
46.49% of the reads were assigned to the forward strand and 43.88% of the reads were assigned to the reverse strand. This indicated that the library was unstranded for the sample.
For GSM461180 – Treated sample
45.24% of the reads were assigned to the forward strand and 45.38% of the reads were assigned to the reverse strand. This indicated that the library was unstranded for the sample.

Counting reads per gene 
Galaxy tool FeatureCounts was used to count the number of reads per annotated gene.

![image](https://user-images.githubusercontent.com/92260802/139520675-1d48747c-a19f-4fcb-a1f2-52a68c135837.png)

![image](https://user-images.githubusercontent.com/92260802/139520686-62218e34-1bde-4b73-8d9f-e2d2e112c635.png)

To determine most feature count in two sample
Sort galaxy tool in descending order

GSM461177
![image](https://user-images.githubusercontent.com/92260802/139520710-d3af47bd-92ca-448d-ac18-9e140e6cee98.png)

GSM461180
![image](https://user-images.githubusercontent.com/92260802/139520722-85e67853-97d9-4726-bba3-d568911eba3c.png)

FBgn0000556 id the feature with most counts in both sample with 128741 counts in untreated and 127416 counts in treated

Summary of quantification can be found [here](https://github.com/S-m-Baffoe/Hackbio_Transcriptomics/blob/main/Counting%20reads.pptx)

## 5. Differential Expression: Deseq2-read count,normalize and extract differentially expressed genes. (Heatmap2-for visualization)
Deseq2 was used to normalize the expression taking into account sequencing depth, gene length and sequencing composition variation in our dataset samples

As initial exploratory analysis on the data a dimension reduction PCA plot was made and it was clear that PC1 could differentiate samples according to their treatment and also PC2 could differentiate samples according to sequencing (paired vs. single end sequencing) this shows that first of all, there was differential gene expression that could clearly cluster treated from untreated samples, but also that sequrncing type affects the differential gene expression abd was therefore important to add in our design model as we did 

![PCAplot](https://user-images.githubusercontent.com/92435273/139514601-d2da908a-26e4-49bc-ad05-8a31c36ed699.png)

Also as QC exploratory check, A distance matrix heatmap gives us an idea about the similarities and dissimilarities between samples where samples are clustered based on distances
![distanceheatmap](https://user-images.githubusercontent.com/92435273/139533645-867b8913-9a7a-4c17-95b9-fab705716162.png)

After this quality check, to investigate differential gene expression an MA plot was made and shows biological significance(log fold-change (logFC) on y axis vs log average expression (logCPM) and significantly differentially expressed genes are shown in blue 

![MAdrosophi](https://user-images.githubusercontent.com/92435273/139533603-287a7182-3682-49ff-b810-442b4616696c.png)

A heatmap for normalized counts for the differentially expressed genes (first top 30 then all 1091 DEGs) was drawn and distinct profiles were observed for treated vs. unreated samples 
![heatmap30](https://user-images.githubusercontent.com/92435273/139533660-271f08d7-6069-4209-be0c-ba05b4c743a3.png)

![heatmap1091](https://user-images.githubusercontent.com/92435273/139533675-8358de00-2136-4757-8cee-96a8f1725555.png)



## 6. Fuctional Enrichment Analysis using goseq

### Gene Ontology analysis
    
Once we have obtained the list of differentially expressed genes between the Drosophila Wildtype and PasillaDepleted samples, the aim is to find out which biological      processes are most affected by these genes. For this purpose, using Galaxy's goseq tool, we performed a gene ontology (GO) analysis for three types of ontology: 
- MF (Molecular Function - molecular activities of gene products)
- CC (Cellular Component - where gene products are active)
- BP (Biological Process - pathways and larger processes made up of the activities of multiple gene products)
    
As in the tutorial, we obtained 31 over-represented Go terms (0.27%) and 83 under-represented Go terms (0.72%) (P-value < 0.05).

Over-represented GO terms:
![image](https://user-images.githubusercontent.com/92274646/139449957-a00378fd-9b84-4910-b2e2-5fa7cc57b76a.png)

Under-represented Go terms:
![image](https://user-images.githubusercontent.com/92274646/139451652-1d8d6aaf-8dab-478c-a05b-b736f2a4604e.png)
(continue...)

In addition, the analysis also returns a graph showing the most over-represented GO categories. In this graph, we observe that more than 60% of the genes of the "blood-brain barrier establishment" caterogy have been identified as differentially expressed. Furthermore, the graph ranks the GO terms according to how significant their involvement is (starting with those with a lower Adj P value). Finally, the larger the size of the circle, the greater the number of genes involved. 

![image](https://user-images.githubusercontent.com/92274646/139452867-5333a357-a0cf-4eef-bb52-864314580dc4.png)

To complete the GO analysis, goseq also provides a list of the genes involved in each GO term in tabular format:
![image](https://user-images.githubusercontent.com/92274646/139453243-4017b1ae-f5f7-43d0-816a-bbcef0bd2d3c.png)

### KEGG Pathway analysis

Similar to the tutorial, the KEGG pathway analysis resulted in 2 over-represented terms which included 

 Over represented terms
 
    1. All metabolic pathways 
    
    
    2. Glycolysis/Gluconeogenesis Pathway
    
   ![KEGG_Pathway_Analysis_Gycolysis_Gluconeogenesis](https://user-images.githubusercontent.com/68198076/139478827-e8538cc8-9fa3-4fa5-a69b-5e59dcd9aa13.png)
    
    




There was one under-represented term observed within the Spliceosome

Under-Represented term

![KEGG_Pathway_Analysis_Spiceosome](https://user-images.githubusercontent.com/68198076/139516559-b6217ddd-d7a8-4fe8-858b-d754edaa6965.png)




## Reference

This work was reproduced from  [Galaxy training on RNA-Seq analysis](https://training.galaxyproject.org/training-material/topics/transcriptomics/tutorials/ref-based/tutorial.html#introduction)


#    Hackbio_Transcriptomics_Team Contributors
###    @miriamdarwish  - Quality Control
###    @shubhangi-chakraborty & Archivicky4 - Mapping
###    @PratikshaBhat - Counting / Quantification
###    @SalmaElShafie - Gene Expression analysis
###    @CrisRisu - Gene Ontology analysis 
###    @SMB - KEGG Pathway analysis


