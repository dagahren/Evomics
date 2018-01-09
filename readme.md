# Sequence Data Quality Control 

## Intro
<!--This document is the embryo of a Bioinformatics Lab for the Workshop on Genomics, Cesky Krumlov, Czechia (yes, the Czech Republic is now referred to as Czechia). 
For more information on the course in its entirety, please have a look at [The Workshop on Genomics 2018](http://evomics.org/workshops/workshop-on-genomics/2018-workshop-on-genomics-cesky-krumlov/) -->

This lab is aimed at giving you a flavour of some of the steps that needs to be taken in most sequencing projects and provides examples from several sequencing technologies. The idea is to give you an feel for how you can handle and process the sequence data that you recieve from the sequencer. Although some parts of the lab and certain tools may be specific to a particular sequencing technology or application, the overall philosophy on handling your sequencing data is the same and has already proven to withstand the test of time.

## Logging in to the virtual machine
Log in to the virtual machine as instructed previously. We will need graphics in this session to view the results files so if you are using ssh, you will need to use the -Y option. 


Before we start there are some notations in the following text that may need a brief introduction.
UNIX commands such as *pwd* will be in italics in the text while commands that you need to type will look like this:
<pre>
 #This is a comment that is not executed
pwd
</pre>
**Note:** In order to give you the chance to solve the exercise on your own the answers/tips are hidden by default. Click on the small arrow to reveal the answer. Try for yourself first and then look at the provided answers to maximise your learning experience!

<details>
<summary> Click to see the hidden answer</summary>
This is the hidden answer! 
</details>  
  
OK, lets get to work! The lab will guide you through a number of important steps and considerations when working with sequence data.

### Datasets
Lets say that you just got your freshly baked sequences back from the sequencing centre. Everyone in the project is excited about the new insights that the data will bring. 
 
<!--Suggestions:
Fungal ITS amplicon PacBio sequence SRR6319114
(Fungal ITS1 Amplicon Illumina SRR6172564)
Sequel data Phanerochaete chrysosporium: SRR5647539


-->
**So what do you do now?**  

This question is the essence of what this lab is all about!

For practical reasons we will use test datasets here, but most steps would apply to your favourite sequencing project.

<!--Provide links and metadata for all datasets used in the exercises below. Make sure link is correct!!-->

All datasets are provided in the following directory:
<pre> 
~/workshop_materials/quality_control/
</pre>

<!--- 
Make sure to make them remember tab-completion, pwd etc
- All data should be gzipped and chmod 444

* Vibrio cholerae H1 strain:H1 Genome sequencing and assembly
* SRR497968 PacBio RS 


* Note Describe ln -s 
* 
 --->



## Quality control

### Why do we need quality control?
While the quality of the sequences we are getting is constantly improving there are a number of reasons why it is important to perform at least some basic quality controls. Modern sequencing technologies can generate a huge number of sequence reads in a single experiment. However, no sequencing technology is perfect, and each instrument will generate different types and amounts of error. Therefore, it is necessary to understand, identify and exclude error types that may impact the interpretation of downstream analysis.
 

- First, you may have recieved bad data and there is a distinct risk that you will waste a lot of time analysing data that are low quality if you do not perform a proper quality control. Perhaps some of the data is even missing from the dataset or adaptors are still in the data.  
- Second, the amount of high quality data should be investigated in order to evaluate whether you have enough data for your project. The amount you require depends on your research question and study design and it is crucial that you get the depth (or more) that you planned for.  
- Third, does the data look like we would expect? This point is not trivial and we will look more carefully into this later in this exercise, but as an example you would not expect the results from your QC from a de novo genome project to look the same as an amplicon project.  
- Forth, are there any biases that you do not expect in your project?  

<!-- - Finally, can you find any evidence of contamination? Is this contamination expected and can be filtered out? Perhaps it is an unexpected biologically interesting finding? Use your scientific skills and knowledge about the system to determine this.  
-->

**Note:** As you probably noticed in the above bullets, the study design and your research project is central to your interpretation of the QC results. It is important to emphasise that you need to treat your bioinformatics project with the same scientific approach as you would a wet lab experiment.

--  
Overview:

* Exercise 1: Bartonella
* Exercise 2: EMP
* Exercise 3: RADsequencing data 
* Exercise 4: Another RAD dataset
* Exercise 5: miSeq Amplicon fastq-mcf
* Exercise 5 smallRNA cutadapt
* Exercise 6: PacBio data 
* Exercise 7: RNA Seq dataset 

#### Exercise 1
**First lets make a directory called qcLab to work in:**
Hint: use mkdir to do this (and only use the hidden answer if you do not manage on your own!

<details>
<summary> Make a directory called qcLab and change into the new directory</summary>
<pre>
mkdir ~/qcLab
cd ~/qcLab
</pre>
</details>  

There are a mulitude of software options for quality control of your sequence reads. Some are R packages such as ShortRead (bioconductor package that can actually do QC, filtering as well as trimming) while others are software that you run in your terminal. 

FastQC is a java based quality control tool that can be used using commands in the terminal or by using a graphical interface. You can run fastqc as a graphical software or directly in the command line. 

* Have a look at the first lines of sequence of the dataset. Can you identify what the different lines in fastq is? Hint: use head and tail. 

* How many reads does the file contain? Use UNIX for counting and compare with what you will get from the FastQC that we will run next.

* Run the fastqc command to start the graphical interface.

<details>
<summary> Start fastqc</summary>
<pre>
fastqc
</pre>
</details>  

It may take a short while to get the graphics presented on your screen so be a bit patient here.

* Next, choose the fastq file you want to investigate. In this case select the gzipped fastq file called **bartonella_illumina.fastq** in the quality\_control directory:

Open->workshop\_materials/quality\_control/bartonella_illumina.fastq


Once you have selected the file, fastqc will start running (it will take a few minutes so a short break is a good idea). 

* Save the report to the qcLab directory.

* Take your time to inspect the results of the FastQC run. Start with the basic statistics table. How many sequences did you get? Compare with your own count above. What is the percent GC? What is the length of the reads?

* Examine the “Overrepresented sequences” page. Why does FastQC give a warning message? Is it a problem for this dataset? 

##### Exercise 2
Now we will look at Illumina sequencing data from the Earth Microbiome Project. This is an environmental sample sequenced for 16S amplicons. This data is in the file EMP_Misc_16v4EMP_NoIndex_L005_R1_001-sample.fastq.gz. 

* Load the data into FASTQC (note that there is no need to unzip it first). Make sure you save the report in the qcLab directory once the results are loaded.
* What can explain the pattern observed in the “Per base sequence content”? 
* What can explain the particular pattern observed at the first 8 bases?
* Based on the warning messages, if this was your data how would you approach the trimming and filtering? Why?

##### Exercise 3
Now we will look at two sets of RAD sequencing data. For this exercise we will use the command line version of FastQC. 

Firstly 12 RAD sequencing samples saved in the compressed folder CAN.tgz.
Uncompress the file using tar:

<details>
<summary> Hint for running tar</summary>
<pre>#Check that you are in the qcLab directory
pwd
cd ~/qcLab 
tar -xzvf ~/workshop_materials/quality_control/CAN.tgz
</pre>
Note that using this command will put the extracted in the directory where you are currently standing in a directory called CAN
</details> 

Analyse each of the samples using FastQC with the command line. This shows how to run it for the first file:
<details>
<summary> Hint for running fastqc command line</summary> 
<pre>fastqc CAN/can152.fq</pre>
Note that by default output of the fastqc will be placed inside the CAN directory.
</details> 

Of course we could run the samples one at a time, but why not use the flexibility of the terminal by using a wildcard symbol to analyse multiple files at once? Try it now and check the results. Did you get fastq outputs for all of them? 

Using your web browser look at the graphical results in the html files. From the command line type for the first file:

<pre>firefox CAN/can152_fastqc.html </pre>

HINT - Think about using the wildcard again.

How can you explain the pattern observed in the first six bases? Why might the beginning of the read be poorer quality?   


##### Exercise 4

Now lets look at a paired-end Illumina RAD dataset. The forward read is in the file KE1_S49_R1_001.fastq.gz and the reverse read is in the KE1_S49_R2_001.fastq. Have a look at the fastq files and the naming of the ID:s in the forward and reverse reads using head and tail. Can you see which reads belong to the same DNA fragment?

* Use fastqc from the command line again to analyse these reads. 
* Again, use firefox to view the reports.
* With Read1, using the “Sequence duplication level”, “Overrepresented sequences” and “Adapter Content” graphs, what can we infer from this data?
* Now look at Read 2, what is different about the “Per base sequence content”. What can we say about this RAD data?
* Still looking at Read 2, take a look at the “Per base N content” and “Overrepresented sequences”. Do you think this is good data?

##### Exercise 5
* We will use cutadapt to filter and trim the data from an Illumina sequencing run of some microRNA. This data can be found in the file 
SRR026762-sample.fastq.gz.

* Analyse the quality of the data using FastQC and make a diagnosis of the data. What is the source of the overrepresented sequences? 
* You are now going to use cutadapt to remove low quality bases and trim the adaptor sequence (these are on the 3’ end). The adaptor used to generate this library was SmallRNA3pAdapter_1.5 ATCTCGTATGCCGTCTTCTGCTTG. This time we have to tell cutadapt the adaptor sequence in the command and not in a file. Again, you’ll need to work out the command to run. To look at the cutadapt manual use:

<pre>cutadapt --help </pre>

You won’t need to unzip the file. Make sure that you specify an output file.

* Analyse the filtered reads with FastQC and make a new diagnosis of the data. What do you observe? Are you satisfied with the outcome?

##### Exercise 6
Let’s look at some Illumina MiSeq paired end sequencing data from an amplicon study - 1_TAAGGCGA-TAGATCGC_L001_R1_001.fastq and 1_TAAGGCGA-TAGATCGC_L001_R2_001.fastq
* Use FastQC to assess the quality (you can use either the graphical user interface or the command line). 

Why is read 2 poorer quality than read 1?

* Now filter the data. For this we will use fastq-mcf. You’re going to need to work out the command to run. Take a look at the manual to tell you the usage statement and the parameter choices:
	<pre>fastq-mcf -h </pre>
	
Run fastq-mcf to filter based on a q score of 35 and a minimum length of 80 bp.
The adaptors can be found in the file adaptors.fasta. Make sure you specify an output file for both the forward and reverse reads with a different name than the raw data. 
Run FastQC on the filtered reads - can you see any changes? 
Think about cases where such strict quality control might not be necessary.


##### Exercise 7

Let look at what PacBio sequences look like. 
These long reads have a higher error rate than Illumina data and a different quality encoding.

* First, have a look at the following file:  m130929_060824_42175_c100588662550000001823089804281482_s1_p0.1.subset.fq
* Use the command line to look at the first five reads. What’s the main difference you notice when looking at PacBio data compared to Illumina data? 
* Analyse the data using FastQC (either GUI or command line). 
Look at the “Per base sequence quality” and the “Sequence Length Distribution”. How do the lengths explain the pattern seen in the “Per base sequence quality” graph?

However, not all PacBio sequences look like this. For example, lets run fastqc on another PacBio dataset. 

* Run fastqc on the PacBio amplicon dataset called SRR6319114.fastq
* Compare the results from the previous PacBio dataset. Why do you see differences in the per base sequence quality and per sequence quality score?

--  

##	Reproducibility 101
We have now run a few analyses with several datasets and programs. Have a look at the files you produced. Would you remember what all the different files are and recall how you produced them? Probably not, so to be kind to your future self finding a way to organise your work is highly recommended. Below there are a few suggestions that could be useful, but feel free to find your own path (no pun intended).Lets do another exercise and this time we will run another exercise with some recommended file structure to follow. It will be a bit harder to keep track of the structure and in which directory you are but for a real project it would be highly beneficial.

Another suggestion is to keep track of commands used by keeping a separate terminal with a text editor of your choice open so that you can document your work as you proceed. There are many ways of doing this (R markdown, jupyter notebook etc), but here I would suggest that you start documenting in a simple text format and save it under the name README in the directory you are working in.

Before starting an analysis it would be a good idea to make your data  read-only using: <pre> chmod 444 *fastq</pre>

OK, so lets make a directory structure worthy of your sequence reads! As already mentioned you can structure this any way you like, this is only a suggested file structure that I have found useful for keeping track of a large number of projects over the years. 
<pre>
mkdir RAD
cd RAD
mkdir Progs Analysis Data Docs Scripts
cd Data
</pre>

In the course we will only need the Analysis and Data directories, but go ahead and make all of the directories anyway.

With the massive amount of sequencing, file sizes and hard disk space is constantly an issue. Therefore is is important to make sure that you do not unnecessarily duplicate large data files. One way to avoid this would be to make symbolic links instead of actually copying. For most practical purposes it does not make any difference in your analyses but you will keep things neat and at the same time avoid wasting disk space.

Here we will reuse the CAN RADSeq data.  

* Make a symbolic link of the CAN datasets in the Data directory

<details>
<summary> Click to see how to make a symbolic link</summary>
<pre>
 ln -s ~/qcLab/CAN/*.fq .
 # This will work because we extracted the CAN.tgz file previously. 
</pre>
</details>  

* Make a directory (*mkdir*) called fastqc in the Analysis directory. The idea is that we want to keep raw data separate from downstream analyses. If you do not remember the structure of the use *pwd*, *ls* and TAB-completion to find your way.

* Run FastQC with the -o option and specify that you want the output in the fastc directory you just created. Remember to run with wildcard (*) to analyse all fastq files in the Data directory.
 
* Make a multiqc directory (*mkdir*, *cd*) in the Analysis directory and run MultiQC to facilitate the comparison between many different samples.
<details>
<summary> Run MultiQC</summary>
<pre>
mkdir ~/qcLab/RAD/Analysis/multiqc
cd ~/qcLab/RAD/Analysis/multiqc
multiqc ../fastqc/
</pre>
</details>  

* Investigate the MultiQC report   
 How does it compare to the FastQC report? Which would you prefer if you had hundreds of RAD samples?

### You have now completed this lab successfully!  
### Well done and good luck with your future projects!

--

 <!--
Questions about the material? Please contact:  
**Dag Ahrén**  
**National Bioinformatics Sweden (NBIS), Lund University, Sweden**  
**Email: dag.ahren@nbis.se**  
Web page: www.nbis.se  
Twitter: @dagahren  
-->
