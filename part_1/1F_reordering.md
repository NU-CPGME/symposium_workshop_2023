# Contig reordering

### April 24, 2024

*Egon A. Ozer, MD PhD (<e-ozer@northwestern.edu>)*  
*Ramon Lorenzo Redondo, PhD (<ramon.lorenzo@northwestern.edu>)* 

----

## Section 1 - Using Mauve to order contigs

Often microbial whole genome assemblies produce fragmented assemblies rather than recreating the entire genome as separate chromosome(s) and/or extrachromosomal elements such as plasmids. This is because short read sequences usually cannot resolve repetitive sequence so the assembler may not be able to determine the order of non-repetitive sequences that are separated by repetitive sequences (tRNA genes, for example). 

There are several software approaches available that will allow you to infer the order of contigs in your assembly if you have a sufficiently closely-related genome sequence to use as a reference. 

### Step 1.1: Activate conda environment

```Shell
conda activate reorder
```

### Step 1.2: Perform alignment

```Shell
progressiveMauve \
  --output=GAS_mauve.xmfa \
  reference/GAS_NGAS638.gbk \
  GAS_assembly/contigs.fasta 
```
**OR** to use the GUI version Mauve (make sure `Mauve` is capitalized) 

```Shell
Mauve
```

1. In the Mauve menu bar, select the following option: _File_ --> _Align with progressiveMauve_
2. Click _Add Sequence..._
3. Select sequence files in this order:  
**1: reference\GAS_NGAS638.gbk**  
**2: GAS_assembly\contigs.fasta**
4. Set output file: GAS_mauve.xmfa
5. Click _Align..._

### Step 1.3: Rearrange contigs relative to a reference sequence

> <img src="../images/warn.png" width="25" /> NOTE: The command below only works if you are using the VirtualBox virtual machine for the workshop. If not, the path to the Mauve.jar file on your computer is going to be different and you should use the GUI instructions instead. 

```Shell
java \
  -Xmx2g \
  -cp /home/workshop/micromamba/envs/reorder/share/mauve-2.4.0.snapshot_2015_02_13-4/Mauve.jar \
  org.gel.mauve.contigs.ContigOrderer \
  -output GAS_contig_mover \
  -ref reference/GAS_NGAS638.gbk \
  -draft GAS_assembly/contigs.fasta
```
**OR** to use the GUI version Mauve (make sure `Mauve` is capitalized) 

```Shell
Mauve
```

1. In the Mauve menu bar, select the options _Tools_ --> _Move Contigs_
2. Create new folder named GAS_contig_mover and select this folder
3. Select sequence files in this order:  
**1: reference\GAS_NGAS638.gbk**  
**2: GAS_assembly\contigs.fasta**  
4. Click _Start_


## Section 2 - Alternative: Ordering contigs with nucmer

### Step 2.1: Activate the conda environment (if you haven't already)

```Shell
conda activate reorder
```

### Step 2.2: Create a new folder for the output and run nucmer to generate an alignment. 

```Shell
mkdir demo_data/GAS_mummer
cd demo_data/GAS_mummer
nucmer ../reference/GAS_NGAS638.fasta ../GAS_assembly/contigs.fasta
```

### Step 2.3: Generate a coverage plot of the contigs vs the reference.

For more information about the mummerplot command, see [here](http://mummer.sourceforge.net/manual/#mummerplot).

```Shell
mummerplot -f -l --png out.delta
```

### Step 2.4: Order contigs relative to the reference and generate pseudomolecule

For more information about the show-tiling command, see [here](http://mummer.sourceforge.net/manual/#tiling).

```Shell
show-tiling -c -p consensus.fasta -u unusable.txt -R -V 0 -v 0 out.delta
```

**Output column definitions:**

Column | Definition
--- | ---
1 | start in the reference 
2 | end in the reference 
3 | gap between this contig and the next 
4 | length of this contig 
5 | alignment coverage of this contig 
6 | average percent identity of this contig 
7 | contig orientation 
8 | contig ID


---

# [Back to table of contents](../README.md)


---
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.





