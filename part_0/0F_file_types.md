# Sequence Data and File Types


### April 24, 2023

*Egon A. Ozer, MD PhD (<e-ozer@northwestern.edu>)*  
*Ramon Lorenzo Redondo, PhD (<ramon.lorenzo@northwestern.edu>)* 

---

There are a certain file types you can expect to encounter consistently when working with sequencing and genomics data. This is not intended to be an exhaustive list, but more if a quick introduction and a reference. 

## Section 1 - [FASTA](https://en.wikipedia.org/wiki/FASTA_format)

FASTA format is the most widely used file format for storing sequence data. 

The basic format of a fasta file is a header line that starts with the greater-than sign ">" followed by identifier information, there can be as much or as little information on this line as you want. Spaces and most special characters are allowed, though be aware some programs will trim after spaces.

The next line and all subsequent lines after the header line contain the sequence associated with the header line. The sequence can all be on one line or split across several lines. Spaces and skipped lines are also allowed.

**FASTA Example 1:**

```
>AB040536.1:142-1209 Streptococcus pyogenes fbaA gene for fibronectin-binding protein, complete cds
ATGCGTAGAGCAGAAAATAACAAACACAGCCGCTATTCCATTCGCAAACTGAGCGTTGGGGTAACGAGTA
TAGCAATTGCGAGTCTCTTTTTAGGAAAGGTTGCCTATGCCGTAGATGGCATCCCTCCAATCTCTCTTAC
TCAAAAGACTACAGCCACTACATCAGAAAATTGGCATCATATTGATAAGGATGGCCTTATTCCTTTAGGT
ATAAGCTTAGAAGCTGCCAAAGAGGAATTTAAAAAAGAAGTAGAAGAATCACGTTTATCTGAAGCACAAA
AAGAAACGTATAAACAAAAAATTAAAACTGCACCAGACAAAGATAAGCTATTATTCACGTATCATAGTGA
GTATATGACAGCCGTTAAGGATCTTCCAGCGTCTACTGAGTCTACTACTCAGCCAGTTGAGGCACCCGTG
CAGGAGACACAGGCATCAGCTTCAGATTCGATGGTGACAGGTGATTCAACATCAGTTACGACTGATTCTC
CTGAGGAAACCCCATCTTCGGAAAGTCCAGTGGCCCCAGCTTTATCTGAGGCTCCAGCTCAACCAGCTGA
GAGTGAGGAACCTTCAGTAGCAGCATCTTCTGAGGAAACCCCATCTCCATCAACTCCAGCAGCCCCATCA
ACTCCAGCGGCTCCAGAAACTCCTGAAGAACCAGCAGCTCCATCTCAACCAGCTGAGAGTGAAGAATCTT
CAGTAGCAGCTACGACAAGCCCGTCTCCATCAACTCCAGCTGAATCAGAGACTCAGACGCCACCAGCTGT
TACTAAAGACTCTGATAAGCCATCTTCAGCAGCTGAAAAACCAGCAGCCTCTTCACTTGTTTCAGAACAA
ACCGTTCAACAACCAACTTCAAAGAGATCTTCTGATAAAAAAGAAGAGCAAGAACAGTCTTACTCTCCAA
ATCGCTCATTGTCAAGACAGGTTAGGGCCCATGAGTCAGGTAAGTACTTGCCTTCAACAGGTGAAAAAGC
ACAGCCACTCTTTATAGCTACTATGACTTTGATGTCTCTATTTGGCAGTCTTTTAGTCACAAAACGCCAA
AAAGAAACTAAAAAATAG
```

A single fasta file can contain multiple separate sequences as long as they are separated by distinct header lines. 

**FASTA Example 2:**

```
>NODE_29_length_255_cov_42.625000
TTTAATTTGCGTTTGAACTTACTCGTTCCTTCTGTCGCTGACAGATTTATTTCTCGTTTC
TTGACGGGTAATATGTCTCCATATCACCCTCACGTTTGGTTCGTCTTATTCAGTTCTCAA
AGGTCTTCTAATCGGGAAGACAGGATTCGAACCTGCGACACCTTGGTCCCAAACCAAGTA
CTCTACCAAGCTGAGCTACTTCCCGAACTGATGCACCCTAGAGGAGTCGAACCTCTAACC
GCCTGATTCGTAGTC
>NODE_30_length_205_cov_19.102564
TCGAACCCGTGTTACCGCCGTGAAAAGGCGGTGTCTTAACCCCTTGACCAACGGACCATA
ATAATATAATTATAGATAATGGGCACGAGTGGACTCGAACCACCGACCTCACGCTTATCA
GGCGTGCGCTCTAACCACCTGAGCTACGCGCCCAAGCTTTTATTGATATAGCTTGGGAAA
ACTATAAAGCGGGTGACGAGAATCG
```

FASTA sequences can contain either nucleotide or amino acid sequences. 

**FASTA Example 3:**

```
>BAB62098.1 fibronectin-binding protein [Streptococcus pyogenes]
MRRAENNKHSRYSIRKLSVGVTSIAIASLFLGKVAYAVDGIPPISLTQKTTATTSENWHHIDKDGLIPLG
ISLEAAKEEFKKEVEESRLSEAQKETYKQKIKTAPDKDKLLFTYHSEYMTAVKDLPASTESTTQPVEAPV
QETQASASDSMVTGDSTSVTTDSPEETPSSESPVAPALSEAPAQPAESEEPSVAASSEETPSPSTPAAPS
TPAAPETPEEPAAPSQPAESEESSVAATTSPSPSTPAESETQTPPAVTKDSDKPSSAAEKPAASSLVSEQ
TVQQPTSKRSSDKKEEQEQSYSPNRSLSRQVRAHESGKYLPSTGEKAQPLFIATMTLMSLFGSLLVTKRQ
KETKK
```

## Section 2 - [FASTQ](https://en.wikipedia.org/wiki/FASTQ_format)

FASTQ files usually contain sequence read data. 

Each individual sequence record in a FASTQ file consists of four lines:

1. Sequence identifier, starts with `@` character
2. Nucleotide sequence
3. Separator line, starts with `+` character
4. Quality values for the sequence in line 2

**FASTQ Example (Two sequence reads):**

```
@M01915:185:000000000-JLJ5N:1:1101:12139:1000 1:N:0:TAAGGCGA+GTAAGGAG
NGTTAGTCTAGCTGGAGAAAAGTCCAGACCGGTTAAACTAAAAGATGTGGATAATATTAGTTATCACAGAACACAGACTG
+
#8ACCGGGGGGGGGGFGGGGGGGGGGGFGC@EGGGGFGGGGGFGFGGGFCGGGFGGGGGGGGFGGGGFGGGGGGGGFFGG
@M01915:185:000000000-JLJ5N:1:1101:17584:1000 1:N:0:TAAGGCGA+GTAAGGAG
NTAACGTCAATTTCCTGCTGATTTCCAAAACGGATAACCATCTGATTTTCTGGAAGATATGGGTCAATAATCGGATCTCC
+
#8ACCGGGGGGGGFGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGFGGGGG<FGGGGGGGGGDG?FFGFGCFFEGGGFGG
```

Each letter in line 4 represents the "Phred" quality score for its corresponding base in line 2. The letters can be translated to values indicating the probability that the corresponding base is incorrectly called. These values are assigned by the sequencing instrument during the sequencing and base-calling process. For more info on Phred scores, see [here](https://en.wikipedia.org/wiki/FASTQ_format).  

## Section 3 - [GenBank](https://www.ncbi.nlm.nih.gov/Sitemap/samplerecord.html)

GenBank-formatted files(.gbk), sometimes referred to as GenBank Flat Files (.gbff), are used for storing detailed annotation data and other metadata about a sequence as well as often the nucleotide sequence itself. These files can contain information about a single gene or an entire genome. 

**GenBank Example 1 (Two sequence reads):**

```
LOCUS       NZ_CP010450          1791401 bp    DNA     circular CON 15-JAN-2022
DEFINITION  Streptococcus pyogenes strain NGAS638 chromosome, complete genome.
ACCESSION   NZ_CP010450
VERSION     NZ_CP010450.1
DBLINK      BioProject: PRJNA224116
            BioSample: SAMN03274510
            Assembly: GCF_001267845.1
KEYWORDS    RefSeq.
SOURCE      Streptococcus pyogenes
  ORGANISM  Streptococcus pyogenes
            Bacteria; Firmicutes; Bacilli; Lactobacillales; Streptococcaceae;
            Streptococcus.
REFERENCE   1  (bases 1 to 1791401)
  AUTHORS   Fittipaldi,N.
  TITLE     Direct Submission
  JOURNAL   Submitted (05-JAN-2015) Public Health Ontario, 661 University Ave,
            Toronto, Ontario M5G 1M1, Canada
FEATURES             Location/Qualifiers
     source          1..1791401
                     /organism="Streptococcus pyogenes"
                     /mol_type="genomic DNA"
                     /strain="NGAS638"
                     /db_xref="taxon:1314"
     gene            join(1790598..1791401,1..3)
                     /locus_tag="SD90_RS00005"
                     /old_locus_tag="SD90_00005"
     CDS             join(1790598..1791401,1..3)
                     /locus_tag="SD90_RS00005"
                     /old_locus_tag="SD90_00005"
                     /inference="COORDINATES: similar to AA
                     sequence:RefSeq:WP_010922805.1"
                     /note="Derived by automated computational analysis using
                     gene prediction method: Protein Homology."
                     /codon_start=1
                     /transl_table=11
                     /product="ParB/RepB/Spo0J family partition protein"
                     /protein_id="WP_011106931.1"
                     /translation="MFLILSRNSLMTKELLIDLPIEDIITNPYQPRIQFNQRELQDLA
                     TSIKSNGLIQPIIVRKSDIFGYELVAGERRLKASKMAGLKKVPAIIKKISTLESMQQA
                     IVENLQRSNLNAIEEAKAYQLLVEKKHMTHDEIAKYMGKSRPYISNTLRLLQLPAPII
                     KAIEEGKISAGHARALLTLSDDKQQLYLTHKIQNEGLSVRQIEQLVTSTPSSKLSKKT
                     KNIFATSLEKQLAKSLGLSVNMKLTANHSGYLQISFSNDDELNRIINKLK"
     gene            236..1591
                     /gene="dnaA"
                     /locus_tag="SD90_RS00010"
                     /old_locus_tag="SD90_00010"
     CDS             236..1591
                     /gene="dnaA"
                     /locus_tag="SD90_RS00010"
                     /old_locus_tag="SD90_00010"
                     /inference="COORDINATES: similar to AA
                     sequence:RefSeq:WP_012657571.1"
                     /note="Derived by automated computational analysis using
                     gene prediction method: Protein Homology."
                     /codon_start=1
                     /transl_table=11
                     /product="chromosomal replication initiator protein DnaA"
                     /protein_id="WP_002987659.1"
                     /translation="MTENEQIFWNRVLELAQSQLKQATYEFFVHDARLLKVDKHIATI
                     YLDQMKELFWEKNLKDVILTAGFEVYNAQISVDYVFEEDLMIEQNQTKINQKPKQQAL
                     NSLPTVTSDLNSKYSFENFIQGDENRWAVAASIAVANTPGTTYNPLFIWGGPGLGKTH
                     LLNAIGNSVLLENPNARIKYITAENFINEFVIHIRLDTMDELKEKFRNLDLLLIDDIQ
                     SLAKKTLSGTQEEFFNTFNALHNNNKQIVLTSDRTPDHLNDLEDRLVTRFKWGLTVNI
                     TPPDFETRVAILTNKIQEYNFIFPQDTIEYLAGQFDSNVRDLEGALKDISLVANFKQI
                     DTITVDIAAEAIRARKQDGPKMTVIPIEEIQAQVGKFYGVTVKEIKATKRTQNIVLAR
                     QVAMFLAREMTDNSLPKIGKEFGGRDHSTVLHAYNKIKNMISQDESLRIEIETIKNKI
                     K"
ORIGIN
        1 tagcttgttg atattctgtt ttttcttttt tagttttcca cataaaaaat agttgaaaac
       61 aatagcggtg tcaccttaaa atgacttttc cacaggttgt ggagaaccca aattaacagt
      121 gttaatttat tttccacaga ttgtggaaaa actaactatt atccattgtt ctgtggaaaa
      181 ctagaatagt ttgtggtaga atagttctag aattatccac aagaaggaac ctagtatgac
      241 tgaaaatgaa caaatttttt ggaacagggt cttggaatta gctcagagtc aattaaaaca
      301 ggcaacttat gaattttttg ttcatgatgc ccgtctatta aaggtcgata agcatattgc
      361 aactatttac ttagatcaaa tgaaagaact cttttgggaa aaaaatctta aagatgttat
      421 tcttactgct ggttttgaag tttataacgc tcaaatttct gttgactatg ttttcgaaga
      481 agacctaatg attgagcaaa atcagaccaa aatcaatcaa aaacctaagc agcaagcctt
      541 aaattctttg cctactgtta cttcagattt aaactcgaaa tatagttttg aaaactttat
      601 tcaaggagat gaaaatcgtt gggctgttgc tgcttcaata gcagtagcta atactcctgg
      661 aactacctat aatcctttgt ttatttgggg tggccctggg cttggaaaaa cccatttatt
      721 aaatgctatt ggtaattctg tactattaga aaatccaaat gctcgaatta aatatatcac
      781 agctgaaaac tttattaatg agtttgttat ccatattcgc cttgatacca tggatgaatt
      841 gaaagaaaaa tttcgtaatt tagatttact ccttattgat gatatccaat ctttagctaa
      901 aaaaacgctc tctggaacac aagaagagtt ctttaatact tttaatgcac ttcataataa
      961 taacaaacaa attgtcctaa caagcgaccg tacaccagat catctcaatg atttagaaga
     1021 tcgattagtt actcgtttta aatggggatt aacagtcaat atcacacctc ctgattttga
     1081 aacacgagtg gctattttga caaataaaat tcaagaatat aactttattt ttcctcaaga
     1141 taccattgag tatttggctg gtcaatttga ttctaatgtc agagatttag aaggtgcctt
     1201 aaaagatatt agtctggttg ctaatttcaa acaaattgac acgattactg ttgacattgc
     1261 tgccgaagct attcgcgcca gaaagcaaga tggacctaaa atgacagtta ttcccatcga
     1321 agaaattcaa gcgcaagttg gaaaatttta cggtgttacc gtcaaagaaa ttaaagctac
     1381 taaacgaaca caaaatattg ttttagcaag acaagtagct atgtttttag cacgtgaaat
     1441 gacagataac agtcttccta aaattggaaa agaatttggt ggcagagacc attcaacagt
     1501 actccatgcc tataataaaa tcaaaaacat gatcagccag gacgaaagcc ttaggatcga
     1561 aattgaaacc ataaaaaaca aaattaaata acatgtggaa aagaatatct tttatgaaat
//   
```

Like FASTA files, GenBank files can also contain information about multiple separate sequences. Information belonging to separate sequences are separated by lines containing two forward slashes `//`
There is nice example GenBank file and detailed explainer at NCBI [here](https://www.ncbi.nlm.nih.gov/Sitemap/samplerecord.html).

## Section 4 - [SAM / BAM](https://samtools.github.io/hts-specs/SAMv1.pdf)

SAM-formatted files are used to store read alignment information. 

In SAM files, each line corresopnds to a single sequence read. Information about the read is organized into columns:

Col | Field | Description
--- | --- | ---
1 | QNAME | Query template name, i.e. read ID
2 | FLAG | Bitwise flag with read mapping information. See [this handy site](https://broadinstitute.github.io/picard/explain-flags.html) for interpreting SAM flags
3 | RNAME | Reference sequence name
4 | POS | Leftmost mapping position of the read on the reference sequence
5 | MAPQ | Mapping quality score
6 | CIGAR | Information about which portions of the read mapped. More detail on CIGAR strings can be found in the [SAM manual](https://samtools.github.io/hts-specs/SAMv1.pdf) or [this brief explainer](https://www.drive5.com/usearch/manual/cigar.html)
7 | RNEXT | Read ID of the paired read (if using paired reads)
8 | PNEXT | Leftmost mapping position of the paired read
9 | TLEN | Template length, i.e. distance between the aligned read pairs
10 | SEQ | Sequence of the read
11 | QUAL | Quality score of the read

Many alignment programs will include extra data about the alignment in columns 12 and up. You should refer to the alignment software's manual for help interpreting these data, if interested.

**SAM Example:**

```
M01915:185:000000000-JLJ5N:1:2117:4014:11709    147     NZ_CP010450     2743    60      85M     =       2743    -85     ACCTTATTGAGTCTTTAAAAGCTATTAAAAGTGAAACAGTAAAAATTCATTTCTTATCACCAGTTCGACCATTCACCCTAACACC   AGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGCCCCC       NM:i:0  MD:Z:85 AS:i:85 XS:i:0  RG:Z:GAS_alignment      MQ:i:60 MC:Z:85M        ms:i:3207
M01915:185:000000000-JLJ5N:1:1115:16084:21280   163     NZ_CP010450     2747    60      100M    =       2747    100     TATTGAGTCTTTAAAAGCTATTAAAAGTGAAACAGTAAAAATTCATTTCTTATCACCAGTTCGACCATTCACCCTAACACCAGGCGATGAGGAAGAAAGT    CCCCCGGGGGGGGFFGGGGGGGFGFGGGGGGGGGGGGGFGFFGGGFGGFGGGGGGFGGGFFFGGGDFGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGFC        NM:i:0  MD:Z:100        AS:i:100        XS:i:0  RG:Z:GAS_alignment      MQ:i:60 MC:Z:100M       ms:i:3766
M01915:185:000000000-JLJ5N:1:1115:16084:21280   83      NZ_CP010450     2747    60      100M    =       2747    -100    TATTGAGTCTTTAAAAGCTATTAAAAGTGAAACAGTAAAAATTCATTTCTTATCACCAGTTCGACCATTCACCCTAACACCAGGCGATGAGGAAGAAAGT    GGGGFGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGFFFFGGGGGGGGGGFEGGGGGGGGGGGGDGGFGGGGGGFFGGGGGGGGGGGGGGGGCCCCC        NM:i:0  MD:Z:100        AS:i:100        XS:i:0  RG:Z:GAS_alignment      MQ:i:60 MC:Z:100M       ms:i:3758
```


BAM files contain the same data as SAM files, but are converted to a binary format to reduce storage requirements as well as for more rapid access by programs that read alignment data. Unlike a SAM file, you won't be able to open and read a BAM file in a text editing program.  

## Section 5 - Tree files

There are a number of differnt formats for storing phylogenetic trees or other relational data. One of the most common and simplest formats is [Newick format](https://evolution.genetics.washington.edu/phylip/newicktree.html). A relative of the Newick tree files is the [Nexus](https://en.wikipedia.org/wiki/Nexus_file) format which contains a newick-formatted tree but can also encode other data associated with the tree. 

Common suffixes for tree files are `.tre` and `.nwk`, but there are others out there.

**Newick Tree File Example:**

```
(COV0536:0.000228603,(COV0118:0.000114239,(COV0074:0.000038079,COV0420:0.000076177)0.997:0.000342946)1.000:0.000342930,(COV1650:0.000114265,COV0415:0.000000005)0.438:0.000000005);
```

---

# [Back to table of contents](../README.md)

---
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.