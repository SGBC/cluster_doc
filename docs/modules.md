# Module System User Guide

## Rationale

On most linux flavors, most software and commands that you use are installed
in `/usr/bin` or `/usr/local/bin`. However, you never have to type the full
path to use them, i.e. you don't type `/usr/bin/cd` but just `cd`!

This is because `/usr/bin` and `/usr/local/bin` are added to an environment
variable: `$PATH`. `$PATH` is a collection of directories where you system
looks for executable software and make them available for you.

Although having the commands you use on a daily basis in your `$PATH` is
convenient, it can become cluttered and confusing if you have too many software
installed, especially the TAB-completion. Since we have a lot of different
Bioinformatics software on our server, we decide to use a module system.

A module system modify your `$PATH` and allow you to cherry-picks which
software should and sould not be in your `$PATH`.

## Browsing available modules

For a list of all software available as modules:

`module av`

For searching a module by partial name or keywords:

`module key $keyword`

For more information about a specific module:

`module whatis $module`

## Loading a module

`module load $module`

which will put all the commands of the software you want in your `$PATH`

Example:

```bash
bowtie2-build --version
# bowtie2-build: command not found
module load bowtie
bowtie2-build --version
# bowtie2-build version 2.2.9
# 64-bit
# Built on localhost.localdomain
# Thu Apr 21 18:36:09 EDT 2016
# Compiler: gcc version 4.1.2 20080704 (Red Hat 4.1.2-54)
# Options: -O3 -m64 -msse2  -funroll-loops -g3 -DPOPCNT_CAPABILITY
# Sizeof {int, long, long long, void*, size_t, off_t}: {4, 8, 8, 8, 8, 8}
```
## Unloading a module

`module unload $module`

## Listing loaded modules

`module list`
