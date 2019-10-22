## Generate samples quality summary data for inspection
The samples quality inspection is required for the HMM training step. As we recommend users to compute their
cohort specific HMM upon their best qualified samples. Once the quality summary data is generated for
each sample, the best samples must be selected according the following parameters:

* BAF_SD: B Allele Frequency standard deviation
* LRR_SD: Log R Ration  standard deviation
* WF: Wave Factor
* Call Rate: Samples Array Genotyping Call Rate

To compute the summary quality data of the cohort, the user must provide to the pipeline the list of all individuals and follow the instruction in the readme.md file. Also, this step of the pipeline can be executed in parallel tasks.

Here is an example of the command line on 10 subjects:

```bash
bash ./cnvCallingPipelineWarper.sh 0 10 0-10 $PWD/PipelineInput.config True False quality > ./outputExamples/output_for_summary_quality_example_10samples.txt
```
The execution last only 15 seconds for the analysis of 10 samples. The output results should looks like the print-screen below:

<p align="center">
  <img src="./_images/output_quality_summary.png" width="800" alt="accessibility text">
</p>

The output results files are located in the provided directory (config file):

```bash
ls /Path_to_the_pipeline_installation_repository/AnalysisScripts_CNVcalling/CNVpennCNV/BATCH_00/LOG_DATA
autosome_sample1.log 
autosome_sample2.log
 ...
autosome_sample10.log
```
Using Linux classic one-liner command lines, one can filter out bad quality samples and keep the best ones 
with at most an LLR_SD value of 0.20 or lower. Why .20 or lower? because PennCNV HMM training default QC only accept
samples quality that passing the indicated threshold.

