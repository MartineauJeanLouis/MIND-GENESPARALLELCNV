<p align="left">
  <img src="./_images/welcomeImage.png" height= "700" width="800" alt="accessibility text">
</p>

<style>
.button {
  display: inline-block;
  border-radius: 4px;
  background-color: #1f2a8d;
  border: none;
  color: #FFFFFF;
  text-align: center;
  font-size: 28px;
  padding: 20px;
  width: 200px;
  transition: all 0.5s;
  cursor: pointer;
  margin: 5px;
}

.button span {
  cursor: pointer;
  display: inline-block;
  position: relative;
  transition: 0.5s;
}

.button span:after {
  content: '\00bb';
  position: absolute;
  opacity: 0;
  top: 0;
  right: -20px;
  transition: 0.5s;
}

.button:hover span {
  padding-right: 25px;
}

.button:hover span:after {
  opacity: 1;
  right: 0;
}
</style>

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.3497400.svg)](https://doi.org/10.5281/zenodo.3497400)

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/MartineauJeanLouis/MIND-GENESPARALLELCNV.git/master)[![CircleCI](https://circleci.com/gh/MartineauJeanLouis/MIND-GENESPARALLELCNV.svg?style=svg)](https://circleci.com/gh/MartineauJeanLouis/MIND-GENESPARALLELCNV)

#### Description
Mind-GenesParallelCNV is freeware tool which mainly implemented to compute CNV calling parallel tasks in the most efficient method. The tool is design to make the command lines as much easier and simple as possible so that researchers form different informatics background level can integrate it in their research project. Also, It has been built to make the parallel tasks possible to be executed on any type of computer including desktops. The tools only work on linux64 for the moment. Other than focusing on calling CNV in parallel, the tool is meant to help detecting CNV using several type of callers based on different algorithms (implementation language may differ between algos). For the moment, the tool generates parallel calls from PennCNV and QuantiSNP, but other CNV caller such as IPattern, BCFtools, FastSeg, DNAcopy, etc will be implemented very soon so that better consensus results can be made available.

As indicated Mind&GenesParallelCNV is a freeware and opensource linux based tool, and users are free to suggest any improvement of it, and eventually digital and intellectual property laws are applied. Therefore, reference should be cited if this tool, any tools from this repo or any crosslinked tools from this repo  have been used in reasearch publication.

To know more about our lab research or our team, please reach the following link:

http://www.minds-genes.org/

#### __Author__:
#### Martineau Jean-Louis

#### __Contact__:
* matineau.jean-louis@umontreal.ca 
* martineau.jean-louis@recherche-ste-justine.qc.ca

This tool is implemented as collaboration to LabJacquemont

Click below to proceed to tutorial

<button class="button" style="vertical-align:middle" onclick="window.location.href = 'https://martineaujeanlouis.github.io/MIND-GENESPARALLELCNV/#/prepare_baff_lrr';"><span>Tutorial </span></button>

