# Hackbio_Transcriptomics
Welcome to the Hackbio Transcriptomics biostack repository



RNA-Seq Analysis Using Galaxy

In this tutorial we identified genes and pathways regulated by the Pasilla gene (the Drosophila homologue of the mammalian splicing regulators Nova-1 and Nova-2 proteins) using RNA-Seq data generated by Brooks et al. 2011.

We compared the genes expression in; 

4 untreated samples: GSM461176, GSM461177, GSM461178, GSM461182

3 treated samples (Pasilla gene depleted by RNAi): GSM461179, GSM461180, GSM461181

Method

## 1. Dataset upload into Galaxy
## 2. Quality Control : FastQC and Cutadapt
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
![4](https://user-images.githubusercontent.com/92271396/139481845-0f4c9a1d-d050-4ca4-8608-026169eb70d6.png)
For one of the sample the percentage of reads mapped to exons is 80% and that to introns is 2.4%
![5](https://user-images.githubusercontent.com/92271396/139481894-5ac47b5e-4885-4037-bd11-b4675855fb29.png)
For another sample the percentage of reads mapped to exons is 81.1% and that to introns is 2%

These results confirm that our sequences were RNA-Seq data, and those were mapped successfully.











 
 







## 4. Quantification: featureCounts
## 5. Differential Expression: Deseq2-read count,normalize and extract differentially expressed genes. (Heatmap2-for visualization)

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

In contrary to the tutorial, the KEGG pathway analysis resulted in 4 over-represented terms which included 

    1. All metabolic pathways 
    
    
    2. Glycolysis/Gluconeogenesis Pathway
    
   ![KEGG_Pathway_Analysis_Gycolysis_Gluconeogenesis](https://user-images.githubusercontent.com/68198076/139478827-e8538cc8-9fa3-4fa5-a69b-5e59dcd9aa13.png)
    
    3. Pentose Phosphate Pathway
    
    4. Glutathione Metabolism
    

Over-represented KEGG terms:


![Over-represented KEGG terms](https://user-images.githubusercontent.com/68198076/139476055-c7b0f725-1f59-45ad-86dc-b249fa0fc5e6.PNG)



There was no under-represented term observed
Under-Represented KEGG terms:
