# Hackbio_Transcriptomics
Welcome to the Hackbio Transcriptomics biostack repository



RNA-Seq Analysis Using Galaxy

In this tutorial we identified genes and pathways regulated by the Pasilla gene (the Drosophila homologue of the mammalian splicing regulators Nova-1 and Nova-2 proteins) using RNA-Seq data generated by Brooks et al. 2011.
We compared the genes expression in; 
4 untreated samples: GSM461176, GSM461177, GSM461178, GSM461182
3 treated samples (Pasilla gene depleted by RNAi): GSM461179, GSM461180, GSM461181

Method
1. Dataset upload into Galazy
2. Quality Control : FastQC and Cutadapt
3. Mapping: RNA STAR visualized using IGV
4. Quantification: featureCounts
5. Differential Expression: Deseq2-read count,normalize and extract differentially expressed genes. (Heatmap2-for visualization)
6. Fuctional Enrichment Analysis using goseq
    Gene Ontology analysis
    GEGG Pathway analysis
