# Introduction
# Experimental Sequencing Pipeline<a name="1"></a>

## 1. Cross-linking of Protein and DNA

## 2. Cell Lysis and Chromatin Shearing

## 3. Immunoprecipitation of DNA-Protein Complexes


## 4. Reverse Cross-linking and DNA Purification

## 5. DNA Sequencing

## 6. Data Analysis

# Bioinformatics Pipeline<a name="2"></a>

After performing ChIP-Seq experiments, the raw data obtained need to be processed through a series of computational steps. Below is a typical pipeline for ChIP-Seq data processing.

## 1. Quality Control of Raw Data<a name="2.1"></a>
Quality control is a crucial part of processing any raw sequencing data, regardless of what type of sequencing data you are working with.
- **Run FastQC**: Run [FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) to assess the quality of the data. FastQC is widely used in other sequencing methods as well. There is a lot of FastQC data to assess, so most importantly, check for the adapter content, per base sequence quality, and sequence duplication levels for good results.
- **Trimming**: If your quality scores are low, consider trimming your data using tools like [fastp](https://github.com/OpenGene/fastp) or [Cutadapt](https://cutadapt.readthedocs.io/en/stable/). Trimming tools are able to remove adapter and low quality sequences from your reads, improving quality scores and reliability of your downstream analyses. 
![image](https://github.com/hbcheng7/BENG183/assets/133923635/80bb6ba8-d960-4306-9626-a42bae78b855)
<br>FASTQC Example Output of Quality Scores
	- In this example, we see a considerable dropoff in quality score toward the end of reads. This tells us that using a trimming tool would be necessary to ensure data quality.

## 2. Alignment to a Reference Genome<a name="2.2"></a>
Alignment is a big step in knowing where all of your raw reads fall. Without it, you won't know where in position these reads are and will have no idea how to piece together the pieces of it. Tools used in this case will give you data on where each read lies in the genome, as well as information about the alignment quality and details. 

- **Alignment Tools**: Map the reads to a reference genome. This can be done with tools such as [Bowtie](https://bowtie-bio.sourceforge.net/manual.shtml), [STAR](https://github.com/alexdobin/STAR), or [BWA](https://bio-bwa.sourceforge.net/bwa.shtml). Your input would be your fastq files, and you will recieve a BAM/SAM file output.
  ![image](https://github.com/hbcheng7/BENG183/assets/133923635/2f8763ed-9b3f-4d0e-96ab-f00274542cb1)
<br>Bowtie Read Alignment Process
  	- In this image, gaps are represented by a red '-' and mismatches are represented with the mismatched nucleotide of the read bolded adn in red. The more mismatches and gaps in the final alignment, the lower the alignment quality tends to be.

- **SAM Format**: It is important to be able to understand your alignment and its details for each read. Thus, you should run SAMtools to generate a SAM format output from a binary BAM format if your aligner returned a BAM file. SAM formats are able to be read and give statistics needed for each read.
![image](https://github.com/hbcheng7/BENG183/assets/130010866/01632628-5b43-4435-a702-0aede747c5cd)
<br>SAM File Format Guide (Read more [here](https://en.wikipedia.org/wiki/SAM_(file_format)))
  
## 3. Post-alignment Processing<a name="2.3"></a>
After alignments, you should run some more quality control filtering. This is mainly to clean up any bad alignments and unneccesary reads. SAMtools can already perform this quality control.

- **Removing Duplicates and Poorly Aligned Reads**: Discard duplicate reads to avoid biases, and throw away poorly aligned reads that will not be useful for the data. SAMtools, SAM Bamba, or Picard Tools are packages that can do this.

## 4. Peak Calling<a name="2.4"></a>
Peak calling is an important step in this process. You want to find where peaks are going to be, and are of a significant higher signal than the background. These will be your areas for where transcription factor binding will be happening. Make sure you normalize your data for neccesary comparisons without bias.
![image](https://github.com/hbcheng7/BENG183/assets/133923635/884969c5-fd2c-46af-b65d-d0e115938c0d)
<br>MACS2 Model For Finding Peaks via Tag Data
  
- **Identify Enriched Peak Regions**: Peak calling can be done with tools such as MACS2 or HOMER.
- **Peak Annotation**: Annotating peaks is important to see which features are most likely to be found at your identified region. This can be done with HOMER or BEDTools. Your results should be genomic signal profiles, distributions of different regulatory elements, and nearest introns/exons/ genic regions.

## 5. Differential Binding Analysis<a name="2.5"></a>
Differential binding analysis is important to visualize the differences in gene regulation that result in different gene expression levels. This will help identify just how different transcription factors bind differently to regions, and how they do so in different scenarios.

- **Comparative Analysis**: Compare different conditions or time points to identify how the binding complex interacts differently. Tools that could be used for this could be DiffBind or MAnorm.
![image](https://github.com/hbcheng7/BENG183/assets/133923635/9d0123cf-ee94-4360-acc1-b09570c98149)
<br>Diffbind binding affinities visualized for each factor

## 6. Visualization<a name="2.6"></a>
It is important to see where in the genome your peaks are being called. This can be done using a genome browser.

- **Genome Browser**: You can visualize the binding sites as peaks compared to the background using tools like the UCSC Genome Browser or IGV.
<img width="663" alt="image" src="https://github.com/hbcheng7/BENG183/assets/133923635/49eae0d5-0f94-4047-a5de-896af21f109c">
<br>IGV Visualization (enriched peaks are circled) 

## 7. Motif Analysis<a name="2.7"></a>
Lastly, you can utilize techniques to find enriched motifs. This allows for us to understand binding preferences and regulatory roles of protein-DNA binding complexes

- **Motif Annotation**: Utilize a tool to discover any motifs and compare them to known background motifs. Usually a list of the most similarly represented motifs will show up, including all the statistics needed such as nucleotide frequencies at each position or probabilities. Tools such as MEME-suite or HOMER can be used.

This pipeline is crucial for transforming raw ChIP-Seq data into meaningful insights about protein-DNA interactions and their impact on gene regulation and cellular processes.

<img width="676" alt="image" src="https://github.com/hbcheng7/BENG183/assets/133923635/a96b9f2c-a98e-41cb-b59d-6cf32b17ef09">
<br>Top Known Motifs Found with their Statistics

# Advantages and Applications
# References
https://www.epicypher.com/resources/blogchromatin-mapping-basics-chipseq/
https://www.cd-genomics.com/pipeline-and-tools-comparison-for-chip-seq-analysis.html
https://hbctraining.github.io/Intro-to-rnaseq-hpc-salmon/lessons/qc_fastqc_assessment.html
https://www.basepairtech.com/blog/basepairs-chip-seq-analysis-pipelines-pathway-enrichment/
