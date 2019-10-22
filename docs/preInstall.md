#### Install Mind&GenesParallelCNV

As most genomics pipelines, this tools require third party software to work.

__- Anaconda, mpi4py__

Anaconda python3 from which mpi4py will be install through conda command line:
Download the linux version of anaconda from the following link:
https://www.anaconda.com/distribution/
then 
``` bash
wget https://repo.anaconda.com/archive/Anaconda3-2019.03-Linux-x86_64.sh && chmod u+rx Anaconda3-2019.03-Linux-x86_64.sh
bash Anaconda3-2019.03-Linux-x86_64.sh
```
and follow the onscreen instructions. For more details on anaconda installation, reach out the tutorial from the link below: https://www.digitalocean.com/community/tutorials/how-to-install-anaconda-on-ubuntu-18-04-quickstart

Now install mpi4py, using conda install:
```bash
conda install -c conda-forge/label/gcc7 mpi4py
```
As indicated in the command line, the compiler for the requested mpi4py is gcc7 and therefore it need to be installed prior to the installation.
 
__- PennCNV__
PennCNV is available through github, and the installation is well explained at the repository.
Visit PennCNV tools at https://github.com/WGLab/PennCNV
Download and install as following:
```bash
git clone https://github.com/WGLab/PennCNV.git
cd PennCNV/kext and make
```
The kext tools are important for hmm training, also swig-3.0.12 or higher is require.
The PennCNV plugins in the main directory are already compile with perl5.

__- QuantiSNP__

The installation of QuantiSNP appear to be more complicated than the other third party tools, also the tool hasn't been maintain since 2008. therefore the user must find his own way if some bugs happened during the installation.

Pre-requisites for QuantiSNP:
> Java JDK8
> GLib 2.6+ as indicated in the tool reference site.
> Matlab compiler runtime (mcr) v7.9

For more installation details, reach their website at: https://sites.google.com/site/quantisnp/ , but don't worry, I will show you output of each step.

```bash
# First create a folder naming as desire
mkdir -p ./QuantiSNP
mkdir -p ./QuantiSNP/MCR
cd QuantiSNP/MCR
wget ftp://ftp.stats.ox.ac.uk/pub/yau/QUANTISNP/mcr/MCRinstaller64.run && chmod u+rx MCRinstaller64.run
bash MCRinstaller64.run
# Follow the onscreen instruction
# It will be asked to download and install ~600Mb of mcr data 
```

```bash
## download b37 folder containing GC5base informations for Hg19 built.
wget ftp://ftp.stats.ox.ac.uk/pub/yau/QUANTISNP/download/b37.tar.gz
tar zxf b37.tar.gz
```

```text
(base) nomade@nomade-Aspire-V3-571:~/QuantiSNP/MCR$ wget ftp://ftp.stats.ox.ac.uk/pub/yau/QUANTISNP/mcr/MCRinstaller64.run
--2019-10-19 14:57:34--  ftp://ftp.stats.ox.ac.uk/pub/yau/QUANTISNP/mcr/MCRinstaller64.run
           => ‘MCRinstaller64.run’
Resolving ftp.stats.ox.ac.uk (ftp.stats.ox.ac.uk)... 163.1.210.245
Connecting to ftp.stats.ox.ac.uk (ftp.stats.ox.ac.uk)|163.1.210.245|:21... connected.
Logging in as anonymous ... Logged in!
==> SYST ... done.    ==> PWD ... done.
==> TYPE I ... done.  ==> CWD (1) /pub/yau/QUANTISNP/mcr ... done.
==> SIZE MCRinstaller64.run ... 318274185
==> PASV ... done.    ==> RETR MCRinstaller64.run ... done.
Length: 318274185 (304M) (unauthoritative)

MCRinstaller64.run                  100%[=================================================================>] 303.53M  14.9MB/s    in 23s

2019-10-19 14:57:58 (13.2 MB/s) - ‘MCRinstaller64.run’ saved [318274185]
```

now, here is the output for the mcr installation.

```text
Verifying archive integrity... All good.
Uncompressing MATLAB Component-Runtime Libraries....

This script will install the MATLAB Component-Runtime Libraries required to run OncoSNP.
Do you wish to proceed with installation of the MCR libraries?
1) Install
2) Quit
#? 1

Installation of the 64-bit MCR Libraries constitutes an agreement to the terms and conditions of the license agreement.
Do you accept the license conditions and still wish to proceed with installation of the MCR libraries? (Y/N)
1) Install
2) View License
3) Quit
#? 1

          Initializing InstallShield Wizard........
          Verifying JVM.
          No Java Runtime Environment(JRE) was found on this system.

Cleaning up installation files
rm: remove write-protected regular file 'license.txt'?
```

This mean Java JRE were not available on the computer, please follow online tutorial on JRE installation for MCR.
In addition, the software doesnt realy need JVM to be pre-installed since it's able to create it's own local copy "QUANTISNP/_JVM/".

Now that MCR v79 is installed, it's time to install the QuantiSNP tool by download and install the file as followed.

```bash
cd ..
wget ftp://ftp.stats.ox.ac.uk/pub/yau/QUANTISNP/executables/09042010/install_quantisnp && chmod u+rx install_quantisnp
bash install_quantisnp
# follow the onscreen instructions
```
QuantiSNP require pre-processed genomics  data which is available on the tool website. User can download them directly from their website. If the genome built data is not available, one can always search them on database sites such as: UCSC genome browser, Ensembl, NCBI and others.

Finally the tool is installed, but it's not the funniest part. The most basic way to test if the installation succeed is to execute the quantisnp executable as followed:
```bash
quantiSNP=./QuantiSNP/quantisnp/linux64/run_QUANTISNP.sh
MCRPATH=./QuantiSNP/v79
bash $quantiSNP $MCRPATH
```

The output should be as followed:


```text
$quantiSNP $MCRPATH
------------------------------------------
Setting up environment variables
---
LD_LIBRARY_PATH is .:./QUANTISNP/v79/runtime/glnxa64:./QUANTISNP/v79/bin/glnxa64:./QUANTISNP/v79/sys/os/glnxa64:./QUANTISNP/v79/sys/java/jre/glnxa64/jre/lib/amd64/native_threads:./QUANTISNP/v79/sys/java/jre/glnxa64/jre/lib/amd64/server:./QUANTISNP/v79/sys/java/jre/glnxa64/jre/lib/amd64/client:./QUANTISNP/v79/sys/java/jre/glnxa64/jre/lib/amd64
QuantiSNP v2.2
----------------


This software was developed and compiled using MATLAB R2008b 
and MATLAB Compiler 4.8 (R) (C) 1984-2010, The Mathworks, Inc.

By using this software you agree to adhere to the terms and conditions
of the licence supplied with this software.


Copyright (c) University of Oxford 2010. Website: http://www.well.ox.ac.uk/QuantiSNP/

-----------------------------------------------------------------------------

QuantiSNP: No output directory specified.
```
## DEBUG QuantiSNP

It may happen sometimes that other libraries or shared libraries are missing. To fix the problem, here is a step by step solution:
* Reinstall QuantiSNP (may fix library export problems)
```text
My Own Exception: Fatal error loading library /home/nomade/Downloads/QUANTISNP/v79/bin/glnxa64/libmwmclmcr.so Error: libXp.so.6: cannot open shared object file: No such file or directory
```
First make sure libmwmclmcrrt.so exists and reinstall quantiSNP from the the executable ** ./QUANTISNP/install_quantisnp.sh **:

```text
find ./QUANTISNP -name "libmwmclmcrrt.so"
./QUANTISNP/v79/runtime/glnxa64/libmwmclmcrrt.so
bash ./QUANTISNP/install_quantisnp.sh
```
* Copy and link required libraries
- Verify the target shared path of __libmwmclmcr.so__
```bash
ldd=QUANTISNP/v79/bin/ldd
ldd QUANTISNP/v79/bin/glnxa64/libmwmclmcr.so
```
The output must show all the shared path required by this library to run properly


```text
linux-vdso.so.1 (0x00007ffdd7fec000)
libmwmcr.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/libmwmcr.so (0x00007f7ef5d5f000)
libut.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/libut.so (0x00007f7ef587c000)
libmwfl.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/libmwfl.so (0x00007f7ef5752000)
libmx.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/libmx.so (0x00007f7ef55e3000)
libmwservices.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/libmwservices.so (0x00007f7ef536b000)
libmex.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/libmex.so (0x00007f7ef525a000)
libmwctfarchiver.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/libmwctfarchiver.so (0x00007f7ef513c000)
libmwctfrt.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/libmwctfrt.so (0x00007f7ef5031000)
libmwctfrtcrypto.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/libmwctfrtcrypto.so (0x00007f7ef4e41000)
libmwm_interpreter.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/libmwm_interpreter.so (0x00007f7ef4716000)
libmwm_dispatcher.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/libmwm_dispatcher.so (0x00007f7ef45b2000)
libmwmpath.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/libmwmpath.so (0x00007f7ef447a000)
libmwjmi.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/libmwjmi.so (0x00007f7ef4326000)
libmwdservices.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/libmwdservices.so (0x00007f7ef41f5000)
libmwudd.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/libmwudd.so (0x00007f7ef400f000)
libboost_regex-gcc41-mt-1_34_1.so.1.34.1 => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/libboost_regex-gcc41-mt-1_34_1.so.1.34.1 (0x00007f7ef3e7f000)
libz.so.1 => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/libz.so.1 (0x00007f7ef3d69000)
libstdc++.so.6 => ./QUANTISNP/v79/bin/glnxa64/../../sys/os/glnxa64/libstdc++.so.6 (0x00007f7ef3b6b000)
libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x00007f7ef37cd000)
libgcc_s.so.1 => ./QUANTISNP/v79/bin/glnxa64/../../sys/os/glnxa64/libgcc_s.so.1 (0x00007f7ef36c0000)
libpthread.so.0 => /lib/x86_64-linux-gnu/libpthread.so.0 (0x00007f7ef34a1000)
libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f7ef30b0000)
libmwiqm.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwiqm.so (0x00007f7ef2f85000)
libmwbridge.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwbridge.so (0x00007f7ef2e5b000)
libmat.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmat.so (0x00007f7ef2d38000)
libmwmlutil.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwmlutil.so (0x00007f7ef2c12000)
libmwm_parser.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwm_parser.so (0x00007f7ef2498000)
libmwmlint.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwmlint.so (0x00007f7ef2334000)
libmwmtok.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwmtok.so (0x00007f7ef222d000)
libmwmcos.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwmcos.so (0x00007f7ef1f13000)
libmwgui.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwgui.so (0x00007f7ef1cd4000)
libmwhg.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwhg.so (0x00007f7ef1941000)
libuij.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libuij.so (0x00007f7ef1804000)
libmwudd_mi.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwudd_mi.so (0x00007f7ef1675000)
libmwuinone.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwuinone.so (0x00007f7ef156f000)
libmwm_ir.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwm_ir.so (0x00007f7ef1423000)
libmwmathutil.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwmathutil.so (0x00007f7ef12ba000)
libmwuix.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwuix.so (0x00007f7ef1047000)
librt.so.1 => /lib/x86_64-linux-gnu/librt.so.1 (0x00007f7ef0e3f000)
libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x00007f7ef0c3b000)
libexpat.so.1 => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libexpat.so.1 (0x00007f7ef0b18000)
libicudata.so.36 => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libicudata.so.36 (0x00007f7ef0a17000)
libicuuc.so.36 => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libicuuc.so.36 (0x00007f7ef07e0000)
libicui18n.so.36 => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libicui18n.so.36 (0x00007f7ef059e000)
libicuio.so.36 => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libicuio.so.36 (0x00007f7ef0492000)
libboost_thread-gcc41-mt-1_34_1.so.1.34.1 => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libboost_thread-gcc41-mt-1_34_1.so.1.34.1 (0x00007f7ef0386000)
libboost_signals-gcc41-mt-1_34_1.so.1.34.1 => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libboost_signals-gcc41-mt-1_34_1.so.1.34.1 (0x00007f7ef0275000)
libncurses.so.5 => /lib/x86_64-linux-gnu/libncurses.so.5 (0x00007f7ef0052000)
libmwprofiler.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwprofiler.so (0x00007f7eeff19000)
libmwmathrng.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwmathrng.so (0x00007f7eefdf4000)
libmwmlib.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwmlib.so (0x00007f7eefce8000)
libmwm_pcodeio.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwm_pcodeio.so (0x00007f7eefbd5000)
libmwm_pcodegen.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwm_pcodegen.so (0x00007f7eefabc000)
libxerces-c.so.27 => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libxerces-c.so.27 (0x00007f7eef5b4000)
libmwdatasvcs.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwdatasvcs.so (0x00007f7eef49a000)
libboost_filesystem-gcc41-mt-1_34_1.so.1.34.1 => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libboost_filesystem-gcc41-mt-1_34_1.so.1.34.1 (0x00007f7eef38d000)
lib64/ld-linux-x86-64.so.2 (0x00007f7ef5ea0000)
libhdf5.so.0 => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libhdf5.so.0 (0x00007f7eef170000)
libmwir_xfmr.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwir_xfmr.so (0x00007f7eef062000)
libmwhardcopy.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwhardcopy.so (0x00007f7eeef32000)
libmwmathlinalg.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwmathlinalg.so (0x00007f7eeed5f000)
libXm.so.3 => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../sys/os/glnxa64/libXm.so.3 (0x00007f7eee9c4000)
libXext.so.6 => /usr/lib/x86_64-linux-gnu/libXext.so.6 (0x00007f7eee7b2000)
libXt.so.6 => /usr/lib/x86_64-linux-gnu/libXt.so.6 (0x00007f7eee549000)
libX11.so.6 => /usr/lib/x86_64-linux-gnu/libX11.so.6 (0x00007f7eee211000)
libXpm.so.4 => /usr/lib/x86_64-linux-gnu/libXpm.so.4 (0x00007f7eedfff000)
libtinfo.so.5 => /lib/x86_64-linux-gnu/libtinfo.so.5 (0x00007f7eeddd5000)
libmwxmlcore.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwxmlcore.so (0x00007f7eedc59000)
libmwmathelem.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwmathelem.so (0x00007f7eeda3e000)
libmwmathcore.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwmathcore.so (0x00007f7eed8f4000)
libmwblas.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwblas.so (0x00007f7eed7de000)
libmwlapack.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwlapack.so (0x00007f7eed61d000)
libmwamd.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwamd.so (0x00007f7eed515000)
libmwcholmod.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwcholmod.so (0x00007f7eed3a1000)
libmwcolamd.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwcolamd.so (0x00007f7eed29b000)
libmwcsparse.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwcsparse.so (0x00007f7eed190000)
libmwma57.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwma57.so (0x00007f7eed06a000)
libmwrookfastbp.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwrookfastbp.so (0x00007f7eecf3a000)
libmwumfpack.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwumfpack.so (0x00007f7eecd96000)
libXmu.so.6 => /usr/lib/x86_64-linux-gnu/libXmu.so.6 (0x00007f7eecb7d000)
libSM.so.6 => /usr/lib/x86_64-linux-gnu/libSM.so.6 (0x00007f7eec975000)
libICE.so.6 => /usr/lib/x86_64-linux-gnu/libICE.so.6 (0x00007f7eec75a000)
libxcb.so.1 => /usr/lib/x86_64-linux-gnu/libxcb.so.1 (0x00007f7eec327000)
libmwbinder.so => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libmwbinder.so (0x00007f7eec220000)
libgfortran.so.1 => ./QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../sys/os/glnxa64/libgfortran.so.1 (0x00007f7eec08c000)
libuuid.so.1 => /lib/x86_64-linux-gnu/libuuid.so.1 (0x00007f7eebe83000)
libbsd.so.0 => /lib/x86_64-linux-gnu/libbsd.so.0 (0x00007f7eebc6e000)
libXdmcp.so.6 => /usr/lib/x86_64-linux-gnu/libXdmcp.so.6 (0x00007f7eeba68000)
libXau.so.6 => /usr/lib/x86_64-linux-gnu/libXau.so.6 (0x00007f7eeb864000)
```

As observed __libXp.so.6__ is not available, and caused the runtime exception. First we need to down load the library file online. My prefered link is https://pkgs.org/.
Make sure to select the proper architecture. If the user doesn't have the proper administrator rights to install the libraries by deb or dpkg, follow the below instructions:
``` text
# make a folder call for ex ExtraLibs
mkdir ./QUANTISNP/ExtraLibs
# download the rpm lib data and proceed as followed:
# Example : libXp-1.0.2-2.1.el6.src.rpm
rpm2cpio ./QUANTISNP/libXp-1.0.2-2.1.el6.src.rpm | cpio -idv
ls ./QUANTISNP
lib/ share/ libXp.spec libXp-1.0.2-2.1.el6.src.rpm
ls ./QUANTISNP/lib/
libXp.a         libXp.la        libXp.so        libXp.so.6      libXp.so.6.2.0  pkgconfig/ 
cp -rf ./QUANTISNP/lib/* ./QUANTISNP/v79/bin/glnxa64
# then execute
ldd QUANTISNP/v79/bin/glnxa64/libmwmclmcr.so | grep "liXp.so"
```

```text
libXpm.so.4 => /usr/lib/x86_64-linux-gnu/libXpm.so.4 (0x00007ff405944000)
libXp.so.6 => /home/nomade/Downloads/QUANTISNP/v79/bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/../../bin/glnxa64/libXp.so.6 (0x00007ff403e96000)
```
Now we see the library shared is properly done between __libXp.so.6__ and __libmwmclmcr.so__. Also we can see that __libXpm.so.4__ were available, therefore we understand that the natif setup of
the OS doesn't comes with the right version of libXpm.so required by QuantiSNP.

That should be all for QuantiSNP installation, for any questions about this installation, don't hesitate to submit a thread on the issue section. A pre-compiled version of QuantiSNP is available, or even better a Docker version (under implementation). Those will be provide only on user request (since the file size are a bit heavy).
 
__- BCFtools CNV calling__

Process under implementation, will be available soon

__- IPattern__

Process under implementation, will be available soon

__- FastSeg__

Process under implementation, will be available soon

__- DNACopy__

Process under implementation, will be available soon

