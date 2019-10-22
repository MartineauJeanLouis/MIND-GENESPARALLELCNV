## Generate PFB per SNP data

The PFB data is required for CNV calling by pennCNV. To compute pfb for a CNV calling project by PennCNV, it exists 2 possibility based on the sample size available. A sample size of the project cohort less than ~300 samples means that there is not enough observation to compute statistically significant population frequency. Therefore the user must download a generic version of the pfb data that reflect the cohort ancestry. In this case, the user can follow the bellow procedure.

The script belong to PennCNV groups authorship name (Leandro Lima lelimaufc@gmail.com) and is available at the PennCNV-seq github repository:
```bash
cd PennCNV-Seq
mkdir /path_to_the_pfb_dataset_downloaded_directory/downloadedGenericPFB
Execute the bash script download_and_format_database.sh by following the help instructions, for example:
./download_and_format_database.sh hg19 1 0
Means that the user chose to download hg19 version of the pfb dataset, (1) choose to split the dataset by chromosome, (0) doesn't want do download the fasta files
```
The download will need to be saved in /path_to_the_pfb_dataset_downloaded_directory/downloadedGenericPFB folder then locate the files with the following pattern "hg38_ALL.sites.2015_08.txt". Since the UCSC dataset doesn't necessairely provide similar SNP name as the commercial ones (Affimetrix, Illumina, etc), it's important that the user match their project SNP names to the downloaded one. 
Here is an example of the download file:
```text
Chr	Position	Ref	Alt	PFB	Name
Y	1085877	G	A	0.000399361	.
Y	1086306	A	C	0.00599042	.
Y	1086318	G	A	0.000399361	.
Y	1086324	T	G	0.000599042	.
Y	1086388	T	G	0.000998403	.
Y	1086395	C	T	0.000199681	.
Y	1086416	G	A	0.000599042	.
Y	1086422	A	G	0.0201677	.
Y	1086430	G	A	0.0229633	.
Y	1086494	G	A	0.0890575	.
```
Here is the expected PFB format:
```text
Name    Chr     Position        PFB
rs116720794     1       729632  0.9712014314928431
rs3131972       1       752721  0.8398232323232327
rs12184325      1       754105  0.9630711422845696
rs3131962       1       756604  0.8488260000000004
rs114525117     1       759036  0.9528859275053313
rs3115850       1       761147  0.8336490280777547
rs115991721     1       767096  0.012686746987951802
rs12562034      1       768448  0.8934478957915828
rs116390263     1       772927  0.9518046092184369
rs4040617       1       779322  0.14156425702811246
```

The user must intersect the project SNP locus data to the downloaded PFB data in other to produce the expect PFB data as formatted above. To do so, one might need the betools intersectbed available at https://github.com/arq5x/bedtools or through apt-get

If the user cohort size is greater than 300 samples, then the user can compute the PFB dataset on his own cohort intensity files. To do so, the user have 2 options computing linearly the analysis without worry about additional files manipulation. The user need to create a list of the raw samples, each preceding their path. The user must not forget to select the best qualified samples (See the CNV calling requirement preparation). Prior the this analysis, the user must compute the summary quality analysis for all the samples then extract the best qualified samples based specific quality threshold criterion (BAF_SD: B Allele frequency standard deviation), LRR_SD: Log R Ratio standard deviation, WF: Absolute value of the wave factor, The sample array call rate(Example are provided graph). 

The user can use the compute_pfb.pl plugins locate in the installation directory of PennCNV, follow the command line below to compute the PFB:

```bash
cd  ~/PennCNV-1.0.5
listOfsampleUnsplited=/path_to_list_of_samplefile/list.txt
outputDir=/path_to_output_pfb_results/
snpFile=/path_to_the_project_SNP_list/SNPlist.txt
./compile_pfb.pl --snpposfile $snpFile --listfile $listOfsampleUnsplited --output $outputDir
```
The above procedure require high memory usage since the matrix of individuals by marker will be loaded in the computer RAM memory, followed by subsequent normalization analysis. This method is not recommended for good productivity analysis. Here we suggest an alternative way is to compute the PFB data which is more efficient than above. The user must fragment the best qualified sample data into chromosomes and remake a list per chromosome for the PFB analysis. Here we provide some scripts which will allow the user to compute pfb analysis task per chromosomes.

the raw input sample data directory should looks like below:
```text
~/path_to_splited_data/chr1/sample_1.chr1.txt
...
~/path_to_splited_data/chr1/sample_n.chr1.txt
.
.
.
~/path_to_splited_data/chrY/sample_1.chrY.txt
...
~/path_to_splited_data/chrY/sample_n.chrY.txt
```
The data dimension become N(samples) x M(chromosomes). Now, per chromosome samples list must be created so that we end up having 24 list of the same number of samples. The use need to name the each list as followed: samplesForPFB.chr1.txt, samplesForPFB.chr2.txt, ..., samplesForPFB.chrY.txt . It's also important to breakdown the marker list file into chromosome subset as followed: markerlist.chr1.list, markerlist.chr2.list, ... , markerlist.chry.list .

To compute the PFB in parallel, the user only need to provide the directory path to the list of splited samples (not the file, only the path), same for the splited marker files, the user will also need to provide the directory path to the save location of the results. The script will search for the input list of splited samples under the format "samplesForPFB.chr*.txt" with the associated splitted marker file then execute the task on a CPU core. At least 24 CPU cores is required for the whole tasks.


Command line to execute the scripts in parallel:
```bash
pyScript=~/MIND-GENESPARALLELCNV/computePFBparallel/pyCNVCallingParallel.py
inputListSamples=/directory_path_to_the_fragmented_list_of_samples_per_chr/
inputListSNP=/directory_path_to_the_fragmented_list_of_SNP_per_chr/
outputResults=/directory_path_to_the_pfb_results_per_chr/
mpirun -np 24 python3 $pyScript $inputListSamples $inputListSNP $outputResults > pfbouput.log
```

