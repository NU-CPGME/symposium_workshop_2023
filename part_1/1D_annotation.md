# Genome annotation

### April 24, 2023

*Egon A. Ozer, MD PhD (<e-ozer@northwestern.edu>)*  
*Ramon Lorenzo Redondo, PhD (<ramon.lorenzo@northwestern.edu>)*   

---
Activate the conda environment:

```Shell
conda activate annotation
```

<img src="../images/warn.png" width="25" /> WARNING: If you are using a Mac, the Conda version of prokka is too buggy for us to use. Instead, use the following commands to install prokka:

If you have [HomeBrew](../part_1/1A_computer_preparation.md#step-3---mac-only-install-xcode-command-line-tools-and-homebrew) installed: 

```Shell
brew install brewsci/bio/prokka
sudo cpan install Bio::SearchIO::hmmer
```

If you don't have HomeBrew:

```Shell
sudo cpan Time::Piece XML::Simple Digest::MD5 Bio::Perl Bio::SearchIO::hmmer
git clone https://github.com/tseemann/prokka.git $HOME/prokka
$HOME/prokka/bin/prokka --setupdb
```

## Section 1 - Genome annotation using prokka

Genome annotation is the process of determining the key features of the genome. This usually incldes identifying gene sequences, but can also include other features such as RNA encoding regions, signal peptides or other important features. The process of annotation is a bit different whether you are working with eukaryotes, prokaryotes, or viruses, but there are a number of programs and applications available to help you with this. 

One of the best annotation applications for bacteria and archaea is the [NCBI Prokaryotic Genome Annotation Pipeline (PGAP](https://www.ncbi.nlm.nih.gov/genome/annotation_prok/). Automatic PGAP annotation can be requested when depositing genome sequences in NCBI and is ultimately their preferred method of annotation. NCBI has made a version of PGAP available for running on demand on your local computer (<https://github.com/ncbi/pgap>), but it is a bit complex to install and maintain. 

If you want quick and useful annotations of bacterial, archeal, or viral genomes you've sequenced and assembled, [prokka](https://github.com/tseemann/prokka) is a pretty good choice.  

**Commands**

```Shell
prokka \
    --outdir GAS_annotation \
    --prefix GAS_example \
    --genus Streptococcus \
    --species pyogenes \
    --proteins reference/GAS_NGAS638.gbk \
    --compliant \
    --cpus 0 \
    GAS_assembly/contigs.fasta
```

**Settings**

Setting | Descripton
--- | ---
`--outdir` | Name of the directory the output files will go into. You'll get an error if this directory already exists
`--prefix` | Name that all of the output files will begin with
`--genus` | Genus ID of the organism
`--species` | Species ID of the organism
`--proteins` | Annotated reference sequence(s) for prokka to use to identify similar proteins in the assembled sequence. Not required, but if you have a closely related reference, this will result in more useful annotations of your assembly. 
`--compliant` | Does some extra filtering to make the annotation "complaint" with NCBI requirements including removing contigs shorter than 200 bp
`--cpus` | Number of compute cores to use. Setting to '0' allows it to detect how many cores your computer has available and use them all

See <https://github.com/tseemann/prokka> more detail on available settings.

**Outputs**

_Adapted from <https://github.com/tseemann/prokka>. Follow the link to see the full list of output files._

| Extension | Description |
| --------- | ----------- |
| .fna | Nucleotide FASTA file of the input contig sequences. |
| .faa | Protein FASTA file of the translated CDS sequences. |
| .ffn | Nucleotide FASTA file of all the prediction transcripts (CDS, rRNA, tRNA, tmRNA, misc_RNA) |
| .gbk | This is a standard Genbank file. If the input to prokka was a multi-FASTA, then this will be a multi-Genbank, with one record for each sequence. |

---

## 5.2 Using command line blast to search your assembly

[BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi) (Basic Local Alignment Search Tool) is one of the most important and most widely used bioinformatic tools. The original publication by Altschul et al "[Basic local alignment search tool](https://doi.org/10.1016/s0022-2836(05)80360-2)" has been cited almost 68,000 times since its publication in 1990. As a tool for searching nucleotide or protein sequences, BLAST is uncontested. 

The web version of the BLAST (<https://blast.ncbi.nlm.nih.gov/Blast.cgi>) has many advantages including a diversity of databases for searching, graphical output, and links to more detailed information on hits, but can suffer from long wait times and limits searchable sequence lengths. Often having the option to perform some BLAST searches on your own computer can be quick and efficient.

Let's say we're interested in whether our assembled genome sequence contains the fibronectin-binding surface protein FbaA, a protein associated with some strains of _Streptococcus pyogenes_ causing skin infections. We can use nucleotide BLAST (blastn) to search our assembly for the nucleotide sequence of the _fbaA_ gene or we can use protein BLAST (blastp) to search the protein sequences from our prokka annotation. BLAST programs have a choice of outputs that can make our searches more efficient, depending on what we're looking for. 

**Commands**

A. Search the genome assembly for the _fbaA_ gene sequence. This will generate the default pairwise output format.

```Shell
blastn \
    -query reference/fbaA.fasta \
    -subject GAS_assembly/contigs.fasta 
```
 
B. Search the prokka annotation for the FbaA protein sequence. Here we'll generate a tabular output format instead.

```Shell
blastp \
    -query reference/FbaA.faa \
    -subject GAS_annotation/GAS_example.faa \
    -outfmt 6
```
 
**Settings**

Setting | Descripton
--- | ---
`-query` | Query sequence in fasta format. This is the sequence you are searching _FOR_.
`-subject` | Subject sequence in fasta format. This is the sequence you are searching _IN_.
`-outfmt` | Output format. The default is '0', i.e. pairwise. '6' produces standard tabular output. 

For more information on settings and how to further customize output formats, run the blast program with the `-help` setting, i.e.

```Shell
blastn -help
``` 

**Outputs**

By default the output of the blast commands will be printed to the screen. If you want to output the results into a file instead, either use the `-out` setting to give an output file or just direct the output to a file using `>`. 

A. Output of `blastn` nucleotide search with pairwise formatting:

```
BLASTN 2.12.0+


Reference: Zheng Zhang, Scott Schwartz, Lukas Wagner, and Webb
Miller (2000), "A greedy algorithm for aligning DNA sequences", J
Comput Biol 2000; 7(1-2):203-14.



Database: User specified sequence set (Input: GAS/contigs.fasta).
           32 sequences; 1,802,596 total letters



Query= AB040536.1:142-1209 Streptococcus pyogenes fbaA gene for
fibronectin-binding protein, complete cds

Length=1068
                                                                      Score     E
Sequences producing significant alignments:                          (Bits)  Value

NODE_4_length_123086_cov_28.457640                                    1973    0.0


> NODE_4_length_123086_cov_28.457640
Length=123086

 Score = 1973 bits (1068),  Expect = 0.0
 Identities = 1068/1068 (100%), Gaps = 0/1068 (0%)
 Strand=Plus/Plus

Query  1       ATGCGTAGAGCAGAAAATAACAAACACAGCCGCTATTCCATTCGCAAACTGAGCGTTGGG  60
               ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
Sbjct  121737  ATGCGTAGAGCAGAAAATAACAAACACAGCCGCTATTCCATTCGCAAACTGAGCGTTGGG  121796

Query  61      GTAACGAGTATAGCAATTGCGAGTCTCTTTTTAGGAAAGGTTGCCTATGCCGTAGATGGC  120
               ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
Sbjct  121797  GTAACGAGTATAGCAATTGCGAGTCTCTTTTTAGGAAAGGTTGCCTATGCCGTAGATGGC  121856

Query  121     ATCCCTCCAATCTCTCTTACTCAAAAGACTACAGCCACTACATCAGAAAATTGGCATCAT  180
               ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
Sbjct  121857  ATCCCTCCAATCTCTCTTACTCAAAAGACTACAGCCACTACATCAGAAAATTGGCATCAT  121916

Query  181     ATTGATAAGGATGGCCTTATTCCTTTAGGTATAAGCTTAGAAGCTGCCAAAGAGGAATTT  240
               ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
Sbjct  121917  ATTGATAAGGATGGCCTTATTCCTTTAGGTATAAGCTTAGAAGCTGCCAAAGAGGAATTT  121976

Query  241     AAAAAAGAAGTAGAAGAATCACGTTTATCTGAAGCACaaaaagaaacgtataaacaaaaa  300
               ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
Sbjct  121977  AAAAAAGAAGTAGAAGAATCACGTTTATCTGAAGCACAAAAAGAAACGTATAAACAAAAA  122036

Query  301     attaaaaCTGCACCAGACAAAGATAAGCTATTATTCACGTATCATAGTGAGTATATGACA  360
               ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
Sbjct  122037  ATTAAAACTGCACCAGACAAAGATAAGCTATTATTCACGTATCATAGTGAGTATATGACA  122096

Query  361     GCCGTTAAGGATCTTCCAGCGTCTACTGAGTCTACTACTCAGCCAGTTGAGGCACCCGTG  420
               ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
Sbjct  122097  GCCGTTAAGGATCTTCCAGCGTCTACTGAGTCTACTACTCAGCCAGTTGAGGCACCCGTG  122156

Query  421     CAGGAGACACAGGCATCAGCTTCAGATTCGATGGTGACAGGTGATTCAACATCAGTTACG  480
               ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
Sbjct  122157  CAGGAGACACAGGCATCAGCTTCAGATTCGATGGTGACAGGTGATTCAACATCAGTTACG  122216

Query  481     ACTGATTCTCCTGAGGAAACCCCATCTTCGGAAAGTCCAGTGGCCCCAGCTTTATCTGAG  540
               ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
Sbjct  122217  ACTGATTCTCCTGAGGAAACCCCATCTTCGGAAAGTCCAGTGGCCCCAGCTTTATCTGAG  122276

Query  541     GCTCCAGCTCAACCAGCTGAGAGTGAGGAACCTTCAGTAGCAGCATCTTCTGAGGAAACC  600
               ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
Sbjct  122277  GCTCCAGCTCAACCAGCTGAGAGTGAGGAACCTTCAGTAGCAGCATCTTCTGAGGAAACC  122336

Query  601     CCATCTCCATCAACTCCAGCAGCCCCATCAACTCCAGCGGCTCCAGAAACTCCTGAAGAA  660
               ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
Sbjct  122337  CCATCTCCATCAACTCCAGCAGCCCCATCAACTCCAGCGGCTCCAGAAACTCCTGAAGAA  122396

Query  661     CCAGCAGCTCCATCTCAACCAGCTGAGAGTGAAGAATCTTCAGTAGCAGCTACGACAAGC  720
               ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
Sbjct  122397  CCAGCAGCTCCATCTCAACCAGCTGAGAGTGAAGAATCTTCAGTAGCAGCTACGACAAGC  122456

Query  721     CCGTCTCCATCAACTCCAGCTGAATCAGAGACTCAGACGCCACCAGCTGTTACTAAAGAC  780
               ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
Sbjct  122457  CCGTCTCCATCAACTCCAGCTGAATCAGAGACTCAGACGCCACCAGCTGTTACTAAAGAC  122516

Query  781     TCTGATAAGCCATCTTCAGCAGCTGAAAAACCAGCAGCCTCTTCACTTGTTTCAGAACAA  840
               ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
Sbjct  122517  TCTGATAAGCCATCTTCAGCAGCTGAAAAACCAGCAGCCTCTTCACTTGTTTCAGAACAA  122576

Query  841     ACCGTTCAACAACCAACTTCAAAGAGATCTTCTGATAAAAAAGAAGAGCAAGAACAGTCT  900
               ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
Sbjct  122577  ACCGTTCAACAACCAACTTCAAAGAGATCTTCTGATAAAAAAGAAGAGCAAGAACAGTCT  122636

Query  901     TACTCTCCAAATCGCTCATTGTCAAGACAGGTTAGGGCCCATGAGTCAGGTAAGTACTTG  960
               ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
Sbjct  122637  TACTCTCCAAATCGCTCATTGTCAAGACAGGTTAGGGCCCATGAGTCAGGTAAGTACTTG  122696

Query  961     CCTTCAACAGGTGAAAAAGCACAGCCACTCTTTATAGCTACTATGACTTTGATGTCTCTA  1020
               ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
Sbjct  122697  CCTTCAACAGGTGAAAAAGCACAGCCACTCTTTATAGCTACTATGACTTTGATGTCTCTA  122756

Query  1021    TTTGGCAGTCTTTTAGTCACaaaacgccaaaaagaaactaaaaaaTAG  1068
               ||||||||||||||||||||||||||||||||||||||||||||||||
Sbjct  122757  TTTGGCAGTCTTTTAGTCACAAAACGCCAAAAAGAAACTAAAAAATAG  122804



Lambda      K        H
    1.33    0.621     1.12

Gapped
Lambda      K        H
    1.28    0.460    0.850

Effective search space used: 1884779032


  Database: User specified sequence set (Input: GAS/contigs.fasta).
    Posted date:  Unknown
  Number of letters in database: 1,802,596
  Number of sequences in database:  32



Matrix: blastn matrix 1 -2
Gap Penalties: Existence: 0, Extension: 2.5
```

B. Tabluar ('6') output of `blastp` search:

```
BAB62098.1	OIOADCLO_01010	100.000	355	0	0	1	355	1	355	0.0	694
BAB62098.1	OIOADCLO_01006	30.357	112	65	4	4	102	2	113	1.93e-04	38.5
BAB62098.1	OIOADCLO_01006	45.000	40	21	1	315	353	445	484	0.003	35.0
BAB62098.1	OIOADCLO_00241	26.174	149	67	5	203	350	184	290	0.003	34.3
BAB62098.1	OIOADCLO_00960	25.974	77	36	2	59	124	350	416	0.051	31.2
BAB62098.1	OIOADCLO_01690	40.000	35	20	1	320	353	183	217	0.084	29.6
BAB62098.1	OIOADCLO_00464	45.833	24	13	0	8	31	3	26	0.40	28.1
BAB62098.1	OIOADCLO_00464	44.737	38	17	2	318	354	1612	1646	4.2	25.0
BAB62098.1	OIOADCLO_01255	47.059	34	17	1	318	350	312	345	0.67	27.3
BAB62098.1	OIOADCLO_01064	42.308	26	15	0	247	272	7	32	1.7	25.4
BAB62098.1	OIOADCLO_00866	25.166	151	89	5	79	229	213	339	2.7	25.4
BAB62098.1	OIOADCLO_01637	33.333	30	20	0	2	31	155	184	4.6	24.6

```

Tabular standard output format:

1. Query ID 
2. Subject ID 
3. Percent identity
4. Alignment length 
5. Number of mismatches 
6. Number of gaps opened 
7. Start of the alignment on the query sequence
8. End of the alignment on the query sequence
9. Start of the alignment on subject sequence 
10. End of the alignment on the subject sequence (if subject start coordinate is greater than subject end coordinate, this indicates the query is aligned on the minus strand of the subject sequence.
11. Expect value (E-value)
12. Bit score

---

# [Back to table of contents](../README.md)

---

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.