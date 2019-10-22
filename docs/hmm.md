## Generate cohort specific HMM data

Now that we have the best quality samples, one can compute the HMM training using the option "hmm". Before launching the analysis, make sure that the list of the best quality samples is already created and specified in the config file. Also one must indicate the location to save the hmm file. This process can not be executed in parallel and can last between 1-2hr for a sample size of ~400 individuals. To start the analysis, follow the command line below:

```bash
./cnvCallingPipelineWarper.sh 0 10 0-10 $PWD/PipelineInput.config True False hmm
```bash

The hmm process example using 10 samples last ~10mn, it saves the results in the resources directory as below:
```bash
/ressources/myPersonalProjectHMM.hmm
/ressources/myPersonalProjectHMM.lrr_baf_pfb
```
The HMM file should looks like the print-screen below.

<p align="center">
  <img src="./_images/HMM.png" width="400" alt="accessibility text">
</p>

The HMM results is a pre-requisite file for PennCNV only, QuantiSNP is able to create it's own EM model each time the CNV calling algorithm is launch. To build a better understand on how and why PennCNV use an HMM model please refer to their tools published paper and repository (http://penncnv.openbioinformatics.org). 

