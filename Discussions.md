Module 4 assignment 1

Alignment Assignment part 1
In this module we will try to follow some of the analysis from this nature paper. In this paper, several transcriptomic analysis on covid patients has been performed to identify possible therapeutic targets for covid-19. The authors have worked on different tissues. We will only focus on peripheral blood mononuclear cells (PBMCs) for now. In this assignment we will prepare all the fastq files required for the differential gene expression analysis.  

1-	Make a new directory and call it “module4”. Run all the analysis in module4 directory.
2-	Make a 00-rawdata directory. From the paper try to find what reference genome is used for alignment of the reads. Download the reference genome in 00-rawdata.
hg38 is the reference genome. I found it online and downloaded it with:
wget 'ftp://hgdownload.soe.ucsc.edu/goldenPath/hg38/bigZips/hg38.fa.gz'

3-	We will be using 3 healthy and 3 covid positive PBMCs samples from the China National Center for Bioinformation’s National Genomics Data Center, https://bigd.big.ac.cn/gsa/browse/CRA002390. From the link find the metadata file. AAs the download process will take some time, I have downloaded them for you and you can use them in CHTC. However. I need you to try to find where you can download the data for all the 6 samples (3 healthy and 3 covid -positive samples) and post a screen shot of where you can download each file from. I need a screen shot with clear link to download. You can draw a box to make it clear what is the download link in the screen shot. 
All of the links are available here. If you click on the blue CRR numbers, that will take you to new pages where there are links for the fastq files:
 
Then on the next page there are the file links (for example):
 

4-	You can access the raw input file at: 

Healthy samples: 

http://proxy.chtc.wisc.edu/SQUID/msayadi/module4/00-rawdata/N1-correct.r1.fastq.gz
http://proxy.chtc.wisc.edu/SQUID/msayadi/module4/00-rawdata/N1-correct.r2.fastq.gz
http://proxy.chtc.wisc.edu/SQUID/msayadi/module4/00-rawdata/N2.r1.fastq.gz
http://proxy.chtc.wisc.edu/SQUID/msayadi/module4/00-rawdata/N2.r2.fastq.gz
http://proxy.chtc.wisc.edu/SQUID/msayadi/module4/00-rawdata/N3.r1.fastq.gz
http://proxy.chtc.wisc.edu/SQUID/msayadi/module4/00-rawdata/N3.r2.fastq.gz

Patient samples: 

http://proxy.chtc.wisc.edu/SQUID/msayadi/module4/00-rawdata/P1.r1.fastq.gz
http://proxy.chtc.wisc.edu/SQUID/msayadi/module4/00-rawdata/P1.r2.fastq.gz
http://proxy.chtc.wisc.edu/SQUID/msayadi/module4/00-rawdata/P2.r1.fastq.gz
http://proxy.chtc.wisc.edu/SQUID/msayadi/module4/00-rawdata/P2.r2.fastq.gz
http://proxy.chtc.wisc.edu/SQUID/msayadi/module4/00-rawdata/P3.r1.fastq.gz
http://proxy.chtc.wisc.edu/SQUID/msayadi/module4/00-rawdata/P3.r2.fastq.gz


5-	Run qc on the 6 input files in 01-qc directory. Post the html file.

qc.sh:
#!/bin/bash
#
# QC 

source /etc/profile

# Install fastqc 
wget https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.11.9.zip
unzip fastqc_v0.11.9.zip
cd FastQC
chmod u=rwx fastqc
cd ..

#get files
wget http://proxy.chtc.wisc.edu/SQUID/msayadi/module4/00-rawdata/N1.r1.fastq.gz
wget http://proxy.chtc.wisc.edu/SQUID/msayadi/module4/00-rawdata/N1.r2.fastq.gz
wget http://proxy.chtc.wisc.edu/SQUID/msayadi/module4/00-rawdata/N2.r1.fastq.gz
wget http://proxy.chtc.wisc.edu/SQUID/msayadi/module4/00-rawdata/N2.r2.fastq.gz
wget http://proxy.chtc.wisc.edu/SQUID/msayadi/module4/00-rawdata/N3.r1.fastq.gz
wget http://proxy.chtc.wisc.edu/SQUID/msayadi/module4/00-rawdata/N3.r2.fastq.gz
wget http://proxy.chtc.wisc.edu/SQUID/msayadi/module4/00-rawdata/P1.r1.fastq.gz
wget http://proxy.chtc.wisc.edu/SQUID/msayadi/module4/00-rawdata/P1.r2.fastq.gz
wget http://proxy.chtc.wisc.edu/SQUID/msayadi/module4/00-rawdata/P2.r1.fastq.gz
wget http://proxy.chtc.wisc.edu/SQUID/msayadi/module4/00-rawdata/P2.r2.fastq.gz
wget http://proxy.chtc.wisc.edu/SQUID/msayadi/module4/00-rawdata/P3.r1.fastq.gz
wget http://proxy.chtc.wisc.edu/SQUID/msayadi/module4/00-rawdata/P3.r2.fastq.gz

#Run FastQC on all files
./FastQC/fastqc  N1.r1.fastq.gz
./FastQC/fastqc  N1.r2.fastq.gz
./FastQC/fastqc  N2.r1.fastq.gz
./FastQC/fastqc  N2.r2.fastq.gz
./FastQC/fastqc  N3.r1.fastq.gz
./FastQC/fastqc  N3.r2.fastq.gz
./FastQC/fastqc  P1.r1.fastq.gz
./FastQC/fastqc  P1.r2.fastq.gz
./FastQC/fastqc  P2.r1.fastq.gz
./FastQC/fastqc  P2.r2.fastq.gz
./FastQC/fastqc  P3.r1.fastq.gz
./FastQC/fastqc  P3.r2.fastq.gz


qc.sub:
universe = vanilla
log = qc.log

should_transfer_files = YES
transfer_output_files = N1.r1_fastqc.html, N1.r2_fastqc.html, N2.r1_fastqc.html, N2.r2_fastqc.html, N3.r1_fastqc.html, N3.r2_fastqc.html, P1.r1_fastqc.html, P1.r2_fastqc.html, P2.r1_fastqc.html, P2.r2_fastqc.html, P3.r1_fastqc.html, P3.r2_fastqc.html

executable = qc.sh
arguments = $(Process)
output = qc_$(Cluster)_$(Process).out
error = qc_$(Cluster)_$(Process).err

requirements = (OpSysMajorVer =?= 7)
request_cpus = 3
request_memory = 50GB
request_disk = 50GB

queue 

 

For some reason, the files for N1.r2, P2.r2, P3.r1, and P3.r2 are empty. There are no obvious errors, so its not clear why these didn’t work. (there was a typo in the wget code from Dr. Sayadi and no P2.r2… but its not clear why the other 3 failed)

Will repeat the code for just those missing files:

qc2.sh:

#!/bin/bash
#
# QC 

source /etc/profile

# Install fastqc 
wget https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.11.9.zip
unzip fastqc_v0.11.9.zip
cd FastQC
chmod u=rwx fastqc
cd ..

#get files
wget http://proxy.chtc.wisc.edu/SQUID/msayadi/module4/00-rawdata/N1.r2.fastq.gz
wget http://proxy.chtc.wisc.edu/SQUID/msayadi/module4/00-rawdata/P2.r2.fastq.gz
wget http://proxy.chtc.wisc.edu/SQUID/msayadi/module4/00-rawdata/P3.r1.fastq.gz
wget http://proxy.chtc.wisc.edu/SQUID/msayadi/module4/00-rawdata/P3.r2.fastq.gz

#Run FastQC on all files
./FastQC/fastqc  N1.r2.fastq.gz
./FastQC/fastqc  P2.r2.fastq.gz
./FastQC/fastqc  P3.r1.fastq.gz
./FastQC/fastqc  P3.r2.fastq.gz


qc2.sub:

universe = vanilla
log = qc.log

should_transfer_files = YES
transfer_output_files = N1.r2_fastqc.html, P2.r2_fastqc.html, P3.r1_fastqc.html, P3.r2_fastqc.html

executable = qc2.sh
arguments = $(Process)
output = qc_$(Cluster)_$(Process).out
error = qc_$(Cluster)_$(Process).err

requirements = (OpSysMajorVer =?= 7)
request_cpus = 3
request_memory = 50GB
request_disk = 50GB

queue 


After running the qc2 file, the P2.r2, P3.r1, and P3.r2 qc analyses seem to have run ok and generated the correct files. The N1.r2 file still was unsuccessful… when looking into the log, it appears that the N1.r2 file is truncated/corrupted.




Trying again to get this N1.r2 qc to work, this time with the downloaded file:

qc3.sh:

#!/bin/bash
#
# QC 

source /etc/profile

# Install fastqc 
wget https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.11.9.zip
unzip fastqc_v0.11.9.zip
cd FastQC
chmod u=rwx fastqc
cd ..

#Run FastQC on all files
./FastQC/fastqc  N1.r2.fastq.gz


qc3.sub:

universe = vanilla
log = qc.log

transfer_input_files = ../02-trimmomatic/N1.r2.fastq.gz
should_transfer_files = YES
transfer_output_files = N1.r2_fastqc.html

executable = qc3.sh
arguments = $(Process)
output = qc_$(Cluster)_$(Process).out
error = qc_$(Cluster)_$(Process).err

requirements = (OpSysMajorVer =?= 7)
request_cpus = 3
request_memory = 50GB
request_disk = 50GB

queue 


THIS DIDN’T WORK EITHER – TRYING NOW WITH THE CORRECTED FILE.

11/4/22 – due to the changed assignment, change code for qc to get the zip files and not just the htmls – code is rerunning (5:29 PM) but not completed yet. 

11/5/22:
Once all of the correct html files are generated, I can run multiqc to combine them into one html:
/squid/msayadi/ProgramFiles/miniconda3/bin/multiqc . 

Then  I can transfer the multiqc .html files to my home node to save:

scp cjstrobel@submit1.chtc.wisc.edu:/staging/groups/UWEx_ABT785_grp/cjstrobel/module4/01-qc/ multiqc_report.html /home/strec7424




6-	Trim the adaptors using Trimmomatic in 02-trimmomatic directory (for all the 6 fastq files)
trim.sh:

#!/bin/bash
#
# Adaptor trimming 

source /etc/profile

# Download 
 wget http://www.usadellab.org/cms/uploads/supplementary/Trimmomatic/Trimmomatic-0.39.zip
unzip Trimmomatic-0.39.zip
 
java -jar Trimmomatic-0.39/trimmomatic-0.39.jar PE -phred33 N1.r1.fastq.gz N1.r2.fastq.gz N1_forward_paired.fq.gz N1_forward_unpaired.fq.gz N1_reverse_paired.fq.gz N1_reverse_unpaired.fq.gz ILLUMINACLIP:Trimmomatic-0.39/adapters/TruSeq3-PE.fa:2:30:10:2:True SLIDINGWINDOW:4:15 MINLEN:36

java -jar Trimmomatic-0.39/trimmomatic-0.39.jar PE -phred33 N2.r1.fastq.gz N2.r2.fastq.gz N2_forward_paired.fq.gz N2_forward_unpaired.fq.gz N2_reverse_paired.fq.gz N2_reverse_unpaired.fq.gz ILLUMINACLIP:Trimmomatic-0.39/adapters/TruSeq3-PE.fa:2:30:10:2:True SLIDINGWINDOW:4:15 MINLEN:36

java -jar Trimmomatic-0.39/trimmomatic-0.39.jar PE -phred33 N3.r1.fastq.gz N3.r2.fastq.gz N3_forward_paired.fq.gz N3_forward_unpaired.fq.gz N3_reverse_paired.fq.gz N3_reverse_unpaired.fq.gz ILLUMINACLIP:Trimmomatic-0.39/adapters/TruSeq3-PE.fa:2:30:10:2:True SLIDINGWINDOW:4:15 MINLEN:36

java -jar Trimmomatic-0.39/trimmomatic-0.39.jar PE -phred33 P1.r1.fastq.gz P1.r2.fastq.gz P1_forward_paired.fq.gz P1_forward_unpaired.fq.gz P1_reverse_paired.fq.gz P1_reverse_unpaired.fq.gz ILLUMINACLIP:Trimmomatic-0.39/adapters/TruSeq3-PE.fa:2:30:10:2:True SLIDINGWINDOW:4:15 MINLEN:36

java -jar Trimmomatic-0.39/trimmomatic-0.39.jar PE -phred33 P2.r1.fastq.gz P2.r2.fastq.gz P2_forward_paired.fq.gz P2_forward_unpaired.fq.gz P2_reverse_paired.fq.gz P2_reverse_unpaired.fq.gz ILLUMINACLIP:Trimmomatic-0.39/adapters/TruSeq3-PE.fa:2:30:10:2:True SLIDINGWINDOW:4:15 MINLEN:36

java -jar Trimmomatic-0.39/trimmomatic-0.39.jar PE -phred33 P3.r1.fastq.gz P3.r2.fastq.gz P3_forward_paired.fq.gz P3_forward_unpaired.fq.gz P3_reverse_paired.fq.gz P3_reverse_unpaired.fq.gz ILLUMINACLIP:Trimmomatic-0.39/adapters/TruSeq3-PE.fa:2:30:10:2:True SLIDINGWINDOW:4:15 MINLEN:36



trim.sub:

universe = vanilla
log = trimmomatic.log

transfer_input_files = N1.r1.fastq.gz, N1.r2.fastq.gz, N2.r1.fastq.gz, N2.r2.fastq.gz, N3.r1.fastq.gz, N3.r2.fastq.gz, P1.r1.fastq.gz, P1.r2.fastq.gz, P2.r1.fastq.gz, P2.r2.fastq.gz, P3.r1.fastq.gz, P3.r2.fastq.gz
should_transfer_files = YES

executable = trim.sh
arguments = $(Process)
output = trim_$(Cluster)_$(Process).out
error = trim_$(Cluster)_$(Process).err

requirements = (OpSysMajorVer =?= 7)
request_cpus = 5
request_memory = 50GB
request_disk = 50GB

queue

11/4/22: Trimmomatic appears to have completed correctly and all files are in the 02-trimmomatic folder:
 
11/5/22:
Repeat trimmomatic on the corrected N1 files:
trim2.sh:

#!/bin/bash
#
# Adaptor trimming 

source /etc/profile

# Download 
 wget http://www.usadellab.org/cms/uploads/supplementary/Trimmomatic/Trimmomatic-0.39.zip
unzip Trimmomatic-0.39.zip
 
java -jar Trimmomatic-0.39/trimmomatic-0.39.jar PE -phred33 N1-correct.r1.fq.gz N1-correct.r2.fq.gz N1_forward_paired.fq.gz N1_forward_unpaired.fq.gz N1_reverse_paired.fq.gz N1_reverse_unpaired.fq.gz ILLUMINACLIP:Trimmomatic-0.39/adapters/TruSeq3-PE.fa:2:30:10:2:True SLIDINGWINDOW:4:15 MINLEN:36



trim2.sub:

universe = vanilla
log = trimmomatic.log

transfer_input_files = N1-correct.r1.fq.gz, N1-correct.r2.fq.gz

executable = trim2.sh
arguments = $(Process)
output = trim_$(Cluster)_$(Process).out
error = trim_$(Cluster)_$(Process).err

requirements = (OpSysMajorVer =?= 7)
request_cpus = 5
request_memory = 50GB
request_disk = 50GB

queue

7-	Run another qc in 03-qcAfterTrim directory. Are the adaptors all trimmed? Post the html file.  Yes it appears that all adapters have been trimmed (see screenshots below) since it says that “no samples found with any adapter contamination > 0.1%”

qcpost.sh
#!/bin/bash
#
# QC post

source /etc/profile

# Install fastqc 
wget https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.11.9.zip
unzip fastqc_v0.11.9.zip
cd FastQC
chmod u=rwx fastqc
cd ..

#Run FastQC on all files
./FastQC/fastqc  N1_forward_paired.fq.gz
./FastQC/fastqc  N1_reverse_paired.fq.gz
./FastQC/fastqc  N2_forward_paired.fq.gz
./FastQC/fastqc  N2_reverse_paired.fq.gz
./FastQC/fastqc  N3_forward_paired.fq.gz
./FastQC/fastqc  N3_reverse_paired.fq.gz
./FastQC/fastqc  P1_forward_paired.fq.gz
./FastQC/fastqc  P1_reverse_paired.fq.gz
./FastQC/fastqc  P2_forward_paired.fq.gz
./FastQC/fastqc  P2_reverse_paired.fq.gz
./FastQC/fastqc  P3_forward_paired.fq.gz
./FastQC/fastqc  P3_reverse_paired.fq.gz

tar -cvf allpostqc.tar *



qcpost.sub
universe = vanilla
log = qcpost.log

transfer_input_files = ../02-trimmomatic/N1_forward_paired.fq.gz, ../02-trimmomatic/N1_reverse_paired.fq.gz, ../02-trimmomatic/N2_forward_paired.fq.gz,  ../02-trimmomatic/N2_reverse_paired.fq.gz, ../02-trimmomatic/N3_forward_paired.fq.gz, ../02-trimmomatic/N3_reverse_paired.fq.gz, ../02-trimmomatic/P1_forward_paired.fq.gz, ../02-trimmomatic/ P1_reverse_paired.fq.gz, ../02-trimmomatic/P2_forward_paired.fq.gz, ../02-trimmomatic/ P2_reverse_paired.fq.gz, ../02-trimmomatic/P3_forward_paired.fq.gz,  ../02-trimmomatic/P3_reverse_paired.fq.gz
should_transfer_files = YES

executable = qcpost.sh
arguments = $(Process)
output = qc_$(Cluster)_$(Process).out
error = qc_$(Cluster)_$(Process).err

requirements = (OpSysMajorVer =?= 7)
request_cpus = 3
request_memory = 50GB
request_disk = 50GB

queue 
 


Once all of the correct html files are generated, I can run multiqc to combine them into one html:
/squid/msayadi/ProgramFiles/miniconda3/bin/multiqc . 

Then  I can transfer the multiqc .html files to my home node to save:

scp cjstrobel@submit1.chtc.wisc.edu:/staging/groups/UWEx_ABT785_grp/cjstrobel/module4/03-qcAfterTrim/ multiqc_report.html /home/strec7424
 
 

8-	Update your GitHub page. 
When you are done with these steps, notify the instructor. After making sure that all the steps are correct, you can move on to the next assignment. 

 

