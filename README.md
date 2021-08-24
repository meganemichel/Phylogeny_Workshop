## MPI-SHH Summer School: Doorway to Human History<br/> Phylogenetics Workshop<br/> 19 Aug. 2021

### Megan Michel<br/> (megan_michel@eva.mpg.de, @megan_michel on Mattermost)

Together we will work through some exercises to gain familiarity with phylogenetic methods used in analyses of ancient pathogen single nucleotide polymorphism (SNP) data. We will be analyzing a dataset of genome-wide SNP data from ancient and modern *Yersinia pestis*, the causative agent of the plague. We will start in breakout rooms and come together at the end of the session to discuss our findings.

## Part 1: Preparation
The first thing you need to do is ensure that you have downloaded the data files and have the correct software installed on your machine.
 
1. **MEGA X (Molecular Genetic Evolutionary Analysis)-** https://www.megasoftware.net/  
Download the version of MEGA X suitable for your machine and follow instructions to install.
 
2. **Data files-** https://github.com/meganemichel/Phylogeny_Workshop  
Download this repository to a convenient location on your local machine. If you are comfortable working with Git on the command line follow the following steps:  
* Open a terminal window on your computer 
* Change into the directory where you would like to save the Phylogeny_Workshop materials  
```
cd /path/to/directory
```
* Clone directory
```
git clone https://github.com/meganemichel/Phylogeny_Workshop.git
```

**Note:** If you are not comfortable working with git, navivate to the URL listed above, select the green "Code button", and choose "Download ZIP". Make sure the unzipped directory (containing the subdirectories Alignments, Metadata, etc.) is named `Phylogeny_Workshop`, and move this directory to the desired location in your filesystem. 

## Part 2: SNP Alignments

Within the directory `Phylogeny_Workshop/Alignments`, you will find a file called `AncModern_pestis.fasta`. This file contains a multiple sequence alignment of ancient and modern *Yersinia pestis* sequences. In the termianl, run the following command to take a look at our alignment file. (Note that /path/to/directory refers to the location on your file system containing 'Phylogeny_Workshop')
```
cd /path/to/directory/Phylogeny_Workshop
head Alignments/AncModern_pestis.fasta 
```
**Note:** If you are running PowerShell on a Windows machine, you can view the first few lines of the file using `gc ./Alighments/AncModern_pestis.fasta | select -first 2`

Each sequence consists of a header line containing the sample name and beginning with '>'. The following line includes a string of nucleotides (**A,T,C,G**). What does **N** stand for in our alignment file?

How many *Y. pestis* sequences are present in our file?
```
grep '>' Alignments/AncModern_pestis.fasta  | wc -l
```
**Note:** If you are running PowerShell on a Windows machine, you can use the command `(Select-String -Path "./Alignments/AncModern_pestis.fasta" -Pattern ">").length`

You might wonder how this alignment was generated in the first place. Just as you learned about in the "Introduction to NGS DNA Sequencing and Processing" session, nf-core/eager was used to preprocess, align, and genotype data from ancient and modern *Y. pestis* strains (https://nf-co.re/eager). Genotyping was performed using the GATK UnifiedGenotyper with diploid calling. Next, MultiVCFAnalzer was used to filter SNPS and generate a SNP alignment (Alexander Herbig, https://github.com/alexherbig/MultiVCFAnalyzer). This SNP alignment was further filetered using a custom R script to exlude positions present in less than 98% of analyzed strains (Aida Andrades Valtueña, https://github.com/aidaanva/MDF). For a more thorough overview of methods used to *Y. pestis* SNP alignments, see Method Details in Andrades Valtueña *et al.* 2017. 

Ok, let's use MEGA X to take a look at this alignment file in more detail. 

Open the MEGA X application. Select the **Data** tab (second from the right) and choose **Open a File/Session...**. First open the complete alignment file (Dataset_genotyped_98_pd.fasta). Select **Analyze**. Next, make sure that **Nucleotide Sequences** is highlighted under **Data Type**. Note what symbols will be used for missing/identical data and alignment gaps. Select **Ok** and choose **No** when asked if this is nucleotide sequence data. 

In the MEGA X Workspace (below the header bar), click on the tab showing the sequence alignment. Use the buttons at the top to explore the alignment in more detail. How many sites are variable? Why do you think this number different from the total number of SNPs included in our alignment file? 

You can also use MEGA X to contruct a pairwise distance matrix. In the toolbar at the top of the MEGA window, select Distance -> Compute Pairwise Distances. Choose Yes when asked whether you would like to use the active data file. In Analysis Preferences, you can select No. of Differences to view the number of SNPs separating each pair of sequences. Alternatively, p-distance gives the proportion of aligned nucleotides at which two sequences differ. All other options can be left at the default settings. If you have questions about any of the other options in Alignment Preferences, you can access MEGA X Help by choosing Help -> Contents or typing Command-H on Mac. 

## Part 3: Contructing Phylogenies

Now that we know how to view our alignment and construct distance matrices, let's build some phylogenies. Select the Phylogeny button from the toolbar and choose a type of phylgony to construct. Your group members may choose to experiment with building different types of phylgenies; alternatively, constructing a Neighbor-Joining tree is a good choice, as it should complete in a very short time. Again, use the active data file and leave Analysis Preferences at default values. Note that we are using Bootstrapping to evaluate the robustness of the phylgeny we infer using a NJ approach. Once MEGA finishes running, you can view the tree that we computed, including a summary of the methods used in it's contruction. You can also experiment with changing the layout/shape of the tree. Locate the Tree Rooting button (third button on the left side) and select *Y. pseudo* (complete name *Y. pseudotuberculosis*) as the outgroup. *Y. psuedotuberculosis* is a close relative of *Y. pestis* that lives in the soil.

Phylogenetic trees provide the most information if we can interpret them in conjunction with detailed strain metadata (such as the geography and time period for ancient samples). To do this we will export our tree into an interactive visualization program called Microreact (https://microreact.org/showcase). 

Once you have a tree you would like to export, choose File-> Export Current Tree (Newick). Tick the Branch Length and Bootstrap boxes to include this data in our exported tree and select ok (loading may take a minute or so). Newick format is common text-based way of representing phylogenies. Choose File -> Save As and save the .nwk file in the Phylogeny folder of the Phylogeny_Workshop directory.

## Part 4: Visualization and Interpretation

Finally, open https://microreact.org/showcase in your favorite webrowser. You can create an free account (My Account) if you would like to save the results of your analysis or continue without an account. Whatever you choose, navigate to the Upload page and choose "browse for files". Select the .nwk file you exported from MEGA. In addition, select the file microreact_data.csv (located in the Phylogeny_Workshop/Metadata directory). This file contains Latitude and Longitude, collection dates, and other associated metadata for the sequences included in our alignment. A final important piece of metadata is the plague outbreak to which each strain belongs (**LNBA** for Late Bronze Age Neolithic, **First Plague** for the Justinianic Plague and Subsequent outbreaks, **Second Plague** for the Black Death and subsequent outbreaks, and **Modern** for the third pandemic and contemporary strains. Take a look at this file in excel to get a better sense of the sequences included in our tree. 

Once both files have been selected, click continue. 

In the resulting window you should see three panes: a map with strain coordinates, a phylogenetic tree, and a timeline. In the timeline panel, ensure that year is selected. Time is displayed in years since the present, so that strains collected today fall at Time 0. Across all three visualizations, points are colored according to the outbreak of the associated strain. Select the eye button at the left side of the screen for a key. Choose the "play button" at the bottom of the screen to view the spread of *Y. pestis* over time and space. 

If you have extra time, compare you phylogeny with those of your neighbors. 