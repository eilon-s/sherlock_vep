# sherlock_vep

This git will build a container with VEP (https://uswest.ensembl.org/info/docs/tools/vep/script/vep_options.html) and data
for Human GRCh37 and Yeast R64-1-1.

If you need data for different genome change this part of the code accordingly:
```
# install data for yeast BY
perl /home/vep/src/ensembl-vep/INSTALL.pl -a acf -s Saccharomyces_cerevisiae -y R64-1-1 -c /home/vep/.vep;
# install data for human hg19
perl /home/vep/src/ensembl-vep/INSTALL.pl -a acf -s homo_sapiens -y GRCh37 -c /home/vep/.vep
```

Notice that building this singularity with Human data take too long to run (few hours) and take considerable space (~50Gb during building and ~12Gb of final .simg file). If you do not need the human data installation remove  it. 
The final .simg file will be a stanalone file of size ~12Gb.

It also install a plugin that is required by pVACseq

## Instructions for building


If you are working with macOS you will follow the following steps (if you are not using macOS you may need just clone git, cd into it and run the build step):

1. Install a virtual machine (VM): https://www.vagrantup.com/
2. Create a working directory and cd to it. Allow >50Gb disk space
3. Clone this git (install git if you need to): git clone https://github.com/eilon-s/sherlock_vep.git. You should see a sherlock_vep directory with a Singularity file (and other files) in it. Only if you need different genomes - modify the singularity file according to the above 
4. Initialize the VM machine:  
```
vagrant init singularityware/singularity-2.4
```
5. Activate the VM
```
vagrant up
```
6. Now you will need to ssh to the VM in order to build the singularity image file in it
```
vagrant ssh
```
7. Now you are working in the virtual machine. The current directory in the host is mapped to /vagrant. So lets cd to the directory in which you have the Singularity file of this git:
```
cd /vagrant/sherlock_vep
```
8. Finally, we are ready to build the Singularity container. This may take few hours. To build the container:
```
sudo singularity build ensembl-vep Singularity
```

## running on Stanford's Sherlock cluster:
1. scp the ensembl-vep to some working directory
2. In order to use Singularity on Sherlock you need to load these modules:
```
module load system;
module load singularity/2.4
```
3. Now you can make sure it is running using:
```
singularity run ensembl-vep --help
```
4. In order to run actual VCF files you need to bind your current directory (though in sherlock this is done by default). Assuming that you have inputfilename.vcf in your current directory, here is a possible command line:
```
singularity run --bind $PWD:$PWD ensembl-vep -i ./inputfilename.vcf -o ./outputfilename --cache --assembly GRCh37 --dir_cache /home/vep/.vep --dir_plugins /home/vep/.plugins --offline --format vcf --vcf --symbol --plugin Downstream --plugin Wildtype --terms SO
```
Notice that the location of the genomes and the plugins

## VEP command line help:

For vep full help see - https://uswest.ensembl.org/info/docs/tools/vep/script/vep_options.html.
This is the short help output:

```
./ensembl-vep 
#----------------------------------#
# ENSEMBL VARIANT EFFECT PREDICTOR #
#----------------------------------#

Versions:
  ensembl              : 91.18ee742
  ensembl-funcgen      : 91.4681d69
  ensembl-io           : 91.923d668
  ensembl-variation    : 91.c78d8b4
  ensembl-vep          : 91.3

Help: dev@ensembl.org , helpdesk@ensembl.org
Twitter: @ensembl , @EnsemblWill

http://www.ensembl.org/info/docs/tools/vep/script/index.html

Usage:
./vep [--cache|--offline|--database] [arguments]

Basic options
=============

--help                 Display this message and quit

-i | --input_file      Input file
-o | --output_file     Output file
--force_overwrite      Force overwriting of output file
--species [species]    Species to use [default: "human"]
                       
--everything           Shortcut switch to turn on commonly used options. See web
                       documentation for details [default: off]                       
--fork [num_forks]     Use forking to improve script runtime

For full option documentation see:
http://www.ensembl.org/info/docs/tools/vep/script/vep_options.html

```

If you have questions, please open an [issue](https://github.com/eilon-s/sherlock_vep/issues).
