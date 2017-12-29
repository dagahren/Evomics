# Inital handling of a sequencing project 

Questions about the material? Please contact:  
**Dag Ahrén**  
**National Bioinformatics Sweden (NBIS), Lund University, Sweden**  
**Email: dag.ahren@nbis.se**  
Web page: www.nbis.se  
Twitter: @dagahren  

## Intro
This document is the embryo of a Bioinformatics Lab for the Workshop on Genomics, Cesky Krumlov, Czechia (yes, the Czech Republic is now referred to as Czechia). 
For more information on the course in its entirety, please have a look at [The Workshop on Genomics 2018](http://evomics.org/workshops/workshop-on-genomics/2018-workshop-on-genomics-cesky-krumlov/)

The parts here are aimed at giving you a flavour of the steps that needs to be taken in most sequencing projects and provides examples from several sequencing technologies. The idea is to give you an feel for how you can handle and process the sequence data that you recieve from the sequencer. Although some parts of the lab and certain tools may be specific to a particular sequencing technology or application, the overall philosophy on handling your sequencing data is the same and has already proven to withstand the test of time.

Before we start there are some notations in the following text that may need a brief introduction.
UNIX commands such as *pwd* will be in italics in the text while commands that you need to type will look like this:
<pre>
\#This is a comment that is not executed
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
**So what do you do now?**  

This question is the essence of what this lab is all about!

For practical reasons we will use test datasets here, but most steps would apply to your favourite sequencing project.

<!--Provide links and metadata for all datasets used in the exercises below. Make sure link is correct!!-->

All datasets are provided in the following directory:
~/workshop_materials/quality_control/

### Software used in the exercises
Seqtk [Seqtk on Github](https://github.com/lh3/seqtk)  
FastQC [FastQC](https://www.bioinformatics.babraham.ac.uk/projects/download.html#fastqc)  
Jellyfish  
MultiQC




##	Reproducibility 101
<!--- Dag will put a limited number of exercises in this section --->

- Organising files in the project (UNIX hygiene): structure/
- A few words on reproducibiliy (be kind to yourself and others)
- Git?
- Datasubmission, Storage vs backup. Save your raw data

Add Exercise 1 here (Making a useful dir structure, write-protecting the raw data, symbolic links. Recommended structure: Data,Docs,Analysis,Scripts,Progs

 
- References
- md5 checksums?
## Quality control

### Why do we need quality control?
While the quality of the sequences we are getting is constantly improving there are a number of reasons why it is important to perform at least some basic quality controls.  

- First, on occasion you may recieve bad data and there is a distinct risk that you will waste a lot of time analysing data that are low quality without noticing. Perhaps some of the data is missing from the dataset?  
- Second, the amount of high quality data should be investigated in order to evaluate whether you have enough data for your project. The amount you require depends on your research question and study design and it is crucial that you get the depth (or more) that you planned for.  
- Third, does the data look like we would expect? This point is not trivial and we will look more carefully into this later in this exercise, but as an example you would not expect the results from your QC from a de novo genome project to look the same as an amplicon project.  
- Forth, are there any biases that you do not expect in your project?  
- Finally, can you find any evidence of contamination? Is this contamination expected and can be filtered out? Perhaps it is an unexpected biologically interesting finding? Use your scientific skills and knowledge about the system to determine this.  

<!--Add something more here?-->

**Note:** As you probably noticed in the above bullets, the study design and your research project is central to your interpretation of the QC results. It is important to emphasise that you need to treat your bioinformatics project with the same scientific approach as you would a wet lab experiment.

--  

#### Exercise 2

There are a mulitude of software options for quality control of your sequence reads. Some are R packages such as ShortRead (bioconductor package that can actually do QC, filtering as well as trimming) while others are software that you run in your terminal. 

##### 2.1 FastQC
Use the structure you created in exercise 1 and make a new directory inside of the the Analysis directory and call it fastqc. Change directory into the fastqc dir you just created. In case you forgot the directory structure that you generated earlier it might be a good idea to use the power of UNIX to refresh your memory a bit (e.g. using *pwd*, *ls* and *cd*)

<details>
<summary> Make a directory called fastqc and change into the new directory</summary>
<pre>
mkdir ~/QClab/Analysis/fastqc
cd ~/QClab/Analysis/fastqc
pwd
</pre>
</details>  

FastQC is a java based quality control tool that can be used using commands in the terminal or by using a graphical interface. 

While still in the fastqc directory run the fastqc command to start the graphical interface.
<details>
<summary> Start fastqc</summary>
<pre>
\#Check that I am in the correct directory
pwd
fastqc
</pre>
</details>  

It may take a short while to get the graphics presented on your screen so be a bit patient here.
Next Choose the fastq file you want to investigate. In this case select the gzipped fastq file called SRR3222409_1.fastq.gz in the Data directory of your project (Remember that this is actually a symbolic link to the real dataset, but it does not matter when we run the fastqc analysis).

Once you have selected the file, fastqc will start running (it will take a few minutes so a short break is a good idea). 

--  

- Think about your research question
- What type of data do you have and what do you expect to get from your sequence provider?
- UNIX calculations. How many reads did you retrieve (check with the output of FASTQC/MultiQC to verify). 
- Kmers Jellyfish? Khmer
- Subfiltering with seqtk
- FastQC Note: explain Q-scores 
- Issues with 2 colour chemistry
- Illumina adapters
- Ribosomal contamination
- Read through issues (adapter and crap sequence
- Drop in base calling accuracy with increasing number of cycles (Illumina, e.g. MiSeq)
- Excercise 2 (MultiQC based, pick examples from the previous lab!)

## Trimming and Filtering
- When to trim and when not to
- Adapter removal
- Bar code issues
- Soft vs hard clipping 
- Removal of complete reads
- Merging overlapping pair-end reads (BBMap?)
- Exercise 3 (MultiQC based, compare with untrimmed data)

## Few last words on data handling
- Summary
- What is important for you and your fellow researchers?
- Rings in the water, inspire others!

<details>
<summary> Click to see how</summary>
This is a hidden answer
</details>  
