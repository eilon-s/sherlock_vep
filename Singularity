Bootstrap: docker
From: willmclaren/ensembl-vep

# sudo singularity build ensembl-vep Singularity

%help
This is a singularity file for VEP docker (v1)	

%environment
    LANGUAGE=en_US
    LANG="en_US.UTF-8"
    LC_ALL=C
    export LANGUAGE LANG LC_ALL

%post
    mkdir -p /home/vep/.vep;
    mkdir -p /home/vep/.plugins;
    perl /home/vep/src/ensembl-vep/INSTALL.pl -a acf -s Saccharomyces_cerevisiae -y R64-1-1 -c /home/vep/.vep;
    perl /home/vep/src/ensembl-vep/INSTALL.pl -a acf -s homo_sapiens -y GRCh37 -c /home/vep/.vep;
    git clone https://github.com/Ensembl/VEP_plugins.git;
    git clone https://github.com/griffithlab/pVAC-Seq.git;
    cp /pVAC-Seq/pvacseq/VEP_plugins/Wildtype.pm /home/vep/.plugins;
    rm -r /pVAC-Seq;
	
%runscript
    exec /home/vep/src/ensembl-vep/vep "$@"
