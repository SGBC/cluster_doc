## Nextflow

[Nextflow](www.nextflow.io) is a domain specific language that "enables scalable and reproducible scientific workflows".

nextflow integraes with the SGE queuing systen and we make it possible for users to run nextflow pipelines on planetsmasher.

### Install nextflow

We recommand to install nextflow in your home directory by executing the following Commands

```bash
mkdir -p ~/.local/bin && cd ~/.local/bin
curl -s https://get.nextflow.io | bash
echo "export PATH=\$HOME/.local/bin:\$PATH" >> ~/.bash_profile
cd
```

Upon disconnecting and reconnecting to the server

```bash
nextflow
```

should print the help on the software on the screen

### Run nextflow pipelines

A few pipeline have been written for our server already.

- [HadrienG/nextflow_prokka](https://github.com/HadrienG/nextflow_prokka): Annotation with prokka

After having checked the required command-line arguments on a pipeline repository, execute it with

```bash
nextflow run PIPELINE ARGUMENTS
```

Per example for the prokka annotation pipeline

```bash
nextflow run HadrienG/nextflow_prokka -profile planet \
    --genome data/aureus.fasta --bioproject PRJEB12345 --genus Staphylococcus \
    --species aureus --strain ST250_MRSA-1 --locus_tag SA0 --taxonomy 1280
```

### Build your own pipeline

When building your own nextflow pipelines, you'll need to give nextfklow some SGE-specific options.
Please find an example configuration file below

```
profiles {
    planet {
        executor = 'sge'

        process.$circlator.clusterOptions = '-S /bin/bash -l h_vmem=5G'
        process.$circlator.time = '30m'
        process.$circlator.penv = 'smp'
        process.$circlator.cpus = 1
        process.$circlator.module = 'circlator'

        process.$prokka.clusterOptions = '-S /bin/bash -l h_vmem=1G'
        process.$prokka.time = '2h'
        process.$prokka.penv = 'smp'
        process.$prokka.cpus = 8
        process.$prokka.module = 'prokka'

        process.$gff3toembl.clusterOptions = '-S /bin/bash -l h_vmem=5G'
        process.$gff3toembl.time = '10m'
        process.$circlator.penv = 'smp'
        process.$gff3toembl.cpus = 1
        process.$gff3toembl.module = 'gff3toembl'
    }
}
```
