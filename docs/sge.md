# SGE User Guide

## Basic SGE Commands

- **qsub** - submit a job script
- **qstat** - monitoring the status of jobs
- **qdel** - delete a job from the queue

## Submit a job

Whether you submit a job interactively or within a script, you always have to
specify a few things (such as the number of cores, amount of RAM and running
time).

**Within a script**:

```shell
#!/usr/bin/env bash

#$ -N my_job    # this is the name of your job
#$ -M e-mail    # your e-mail address
#$ -cwd         # use the directory you're running from
#$ -l h_rt=240:0:0,h_vmem=1G    # running time in hours:min:sec and memory (per cpu) required for the job
#$ -pe smp 4    # number of cpus for the job
#$ -j y         # joining the output from standard out and standard error to one file

... here goes the actual code...
```

then submit the script using `qsub my_job.sh`

**Interactively**:

```shell
qsub -N my_job -j y -cwd -l h_vmem=100M -l h_rt=0:15:0 -pe smp 4 my_job.sh
```

### Job parameters

    -cwd                                   use current working directory

Use your current working directory for relative paths used in job

    -e path_list                           specify standard error stream path(s)

If you don't wish to join stderr and stdout to one file, you can specify their
path using the `-e` and `-o` options

    -j y[es]|n[o]                          merge stdout and stderr stream of job

If used, `-j y` will merge stdout and stderr to one file

    -l resource_list                       request the given resources

This option is used to request the RAM and time allocated to the job. Use
`h_vmem=1G` to use 1G of RAM per cpu (accepts prefixes `M` and `G`). Use
`h_rt=hours:minutes:seconds` to specify the maximum time allocated to your job.

    -M mail_list                           notify these e-mail addresses

Use this option if you wish to be notified by email at the end of your job

    -m mail_options                        define mail notification events

Use `-m` to be notified of job events. Parameter is a combination of the
following letters:
`b` - Mail is sent at the beginning of the job.
`e` - Mail is sent at the end of the job.
`a` - Mail is sent when the job is aborted or rescheduled.
`s` - Mail is sent when the job is suspended.
`n` - No mail is sent.

    -N name                                specify job name

The name you wish to give to your job

    -o path_list                           specify standard output stream path(s)

See `-e`

    -pe smp slot_range                     request slot range for parallel jobs

How many cpus you want to use. Can be an integer (`-pe samp 4`) or a range
(`-pe smp 1-4`). In case of a range the job will use cores up to the upper
bound if available

    -q wc_queue_list                       bind job to queue(s)

Run on a specific node

    -v variable_list                       export these environment variables

export existing variables

    -verify                                do not submit, just verify

similar to a dry-run, "test your job"

    -V                                     export all environment variables

export all current environment variables to your job

    -@ file                                read commandline input from file

## Monitoring job status

Simple! Just use `qstat` to see the state your jobs in the queue.

If you wish to see the whole queue, use `qstat -u \*`

For detailed information about one job, use `qstat -j job_number`

## Deleting jobs

Use the qdel command for deleting jobs:

`qdel job_id`
