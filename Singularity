Bootstrap: docker
From: willmclaren/ensembl-vep

# sudo singularity build ensembl-vep Singularity

%help
	This is a singularity file for VEP docker.
	run example: singularity run --bind $PWD:$PWD ensembl-vep -i ./inputfilename.vcf -o ./outputfilename --cache --assembly GRCh37 --dir_cache /home/vep/.vep --dir_plugins /home/vep/.plugins --offline --format vcf --vcf --symbol --plugin Downstream --plugin Wildtype --terms SO

%environment
    LANGUAGE=en_US
    LANG="en_US.UTF-8"
    LC_ALL=C
    export LANGUAGE LANG LC_ALL

%post
    mkdir -p /home/vep/.vep;
    mkdir -p /home/vep/.plugins;
    # install data for yeast BY
    perl /home/vep/src/ensembl-vep/INSTALL.pl -a acf -s Saccharomyces_cerevisiae -y R64-1-1 -c /home/vep/.vep;
    # install data for human hg19
    perl /home/vep/src/ensembl-vep/INSTALL.pl -a acf -s homo_sapiens -y GRCh37 -c /home/vep/.vep
    # get plugins
    git clone https://github.com/Ensembl/VEP_plugins.git;
    cp -r /VEP_plugins/* /home/vep/.plugins;
    rm /VEP_plugins;
    # get pVACseq plugin
    git clone https://github.com/griffithlab/pVAC-Seq.git;
    cp /pVAC-Seq/pvacseq/VEP_plugins/Wildtype.pm /home/vep/.plugins;
    rm -r /pVAC-Seq;
    
%runscript
    exec /home/vep/src/ensembl-vep/vep "$@"
