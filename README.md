# tnseq-GI

Tool for analysis of genetic interactions (GI) from TnSeq data.


#### <a name="version">Version History</a>

**Versionn 1.1.0 changes**
 - Using empirical priors
 - Added type classification in output;
 - More options in arguments


#### <a name="requirements">Requirements:</a>

*   Python 2.7+ [www.python.org](http://www.python.org)
*   Scipy 0.6.0+ [www.scipy.org/Download](http://www.scipy.org/Download)
*   Numpy 1.2.1+ [www.scipy.org/Download](http://www.scipy.org/Download)


## <a name="source">Data</a>

Example files are provided below to test the execution of the script and help verify that input files are in the appropriate format:

*   Reference strain
    *   [H37Rv Day 0 replicate #1](H37Rv_day0_rep1.wig)
    *   [H37Rv Day 0 replicate #2](H37Rv_day0_rep2.wig)
    *   [H37Rv Day 32 replicate #1](H37Rv_day0_rep2.wig)
    *   [H37Rv Day 32 replicate #2](H37Rv_day0_rep2.wig)
    *   [H37Rv Day 32 replicate #3](H37Rv_day0_rep2.wig)
*   Knockout strain
    *   [Rv2680-KO Day 0 replicate #1](Rv2680_day0_rep1.wig)
    *   [Rv2680-KO Day 0 replicate #2](Rv2680_day0_rep2.wig)
    *   [Rv2680-KO Day 32 replicate #1](Rv2680_day32_rep1.wig)
    *   [Rv2680-KO Day 32 replicate #2](Rv2680_day32_rep2.wig)
    *   [Rv2680-KO Day 32 replicate #3](Rv2680_day32_rep3.wig)
*   [H37Rv annotation](H37Rv.prot_table)

## <a name="instructions">Instructions</a>

Before running the python script, please make sure you have installed all the necessary [prerequisites](#requirements) listed above, and have downloaded the latest version of the script. Once the prerequisite software and libraries are installed, to run the script simply type the following command:

<pre>python tnseq_GI.py -wt1 H37Rv_d0_r1.wig,H37Rv_d0_r2.wig -wt2 H37Rv_d32_r1.wig,H37Rv_d32_r2.wig -ko1 Rv260_KO_d0_r1.wig,Rv260_KO_d0_r2.wig -ko2 Rv2680_KO_d32_r1.wig,Rv2680_KO_d32_r2.wig -pt H37Rv.prot_table
</pre>

Below the [input file formats](#format) are described, followed by description of the [input flags](#flags) available, and finally a description of the [output format](#output) for the results.

#### <a name="format">Input File Formats</a>

Read data must be contained in a text-file according to the [WIG format](https://genome.ucsc.edu/goldenpath/help/wiggle.html). This format has two space-delimited representing the position of all insertion sites (i.e. including ones where no reads were mapped), and the read count information at that site as shown below:  

<pre># Generated by tpp from U19_75_R1.fastq and U19_75_R2.fastq
variableStep chrom=H37Rv
60 0
72 0
102 0
188 0
246 0
333 0
360 0
426 0
</pre>

#### <a name="flags">Flags</a>

The table below outlines the flags accepted by the python script:  

<table>

<tbody>

<tr>

<th width="5%" align="left">Flag</th>

<th width="10%" align="left">Value</th>

<th width="85%" align="left">Definition</th>

</tr>

<tr>

<td>-wt1</td>

<td>[String]</td>

<td>Comma separated list of paths to WIG formatted datasets for the reference strain under the first condition. Example: -wt1 H37Rv_d0_r1.wig,H37Rv_d0_r2.wig</td>

</tr>

<tr>

<td>-wt2</td>

<td>[String]</td>

<td>Comma separated list of paths to WIG formatted datasets for the reference strain under the second condition. Example: -wt1 H37Rv_d32_r1.wig,H37Rv_d32_r2.wig</td>

</tr>

<tr>

<td>-ko1</td>

<td>[String]</td>

<td>Comma separated list of paths to WIG formatted datasets for the knockout strain under the first condition. Example: -wt1 Rv2680_KO_d0_r1.wig,Rv2680_KO_d0_r2.wig</td>

</tr>

<tr>

<td>-ko2</td>

<td>[String]</td>

<td>Comma separated list of paths to WIG formatted datasets for the knockout strain under the second condition. Example: -wt1 Rv2680_KO_d32_r1.wig,Rv2680_KO_d32_r2.wig</td>

</tr>

<tr>

<td>-pt</td>

<td>[String]</td>

<td>Path to the annotation file in prot_table format or GFF3 format. Example: -pt H37Rv.prot_table</td>

</tr>

<tr>

<td>-s</td>

<td>[Integer]</td>

<td>Number of samples to take for estimate of posterior distributions. Default: -s 20000</td>

</tr>

<tr>

<td>-rope</td>

<td>[Float]</td>

<td>+/- Window to define Region of Potential Equivalency (ROPE) i.e. the region that defines non-interaction. Default: -rope 0.5</td>

</tr>

<tr>

<td>--debug</td>

<td>[String]</td>

<td>Comma-separated list of ORF IDs. Limits analysis to those genes and outputs more information. Useful for debugging.</td>

</tr>

</tbody>

</table>

#### <a name="output">Output Format</a>

Results are printed to screen in tab-separated format:  

<pre># Copyright 2016\. Michael A. DeJesus & Thomas R. Ioerger
# Version 1.00; http://saclab.tamu.edu/essentiality/GI
#
# python tnseq_GI.py -wt1 H37Rv_day0_rep1.wig -wt2 H37Rv_day32_rep1.wig -ko1 Rv2680_day0_rep1.wig -ko2 Rv2680_day32_rep1.wig -pt H37Rv.prot_table
# mu0=18.02, S=20000, s20=1.0, k0=1.0, nu0=2.0
# ROPE: 0.5
#orf    Name    Description N   Mean WT-1   Mean WT-2   Mean KO-1   Mean KO-2   Mean logFC WT-2/WT-1    Mean log FC KO-2/KO-1   Mean delta logFC    L. Bound    U. Bound    Outside of HDI?
Rv1473  -   PROBABLE MACROLIDE-TRANSPORT ATP-BINDING PROTEIN ABC TRANSPORTER    25  4.40    0.08    1.56    11.41   -3.95   2.43    6.38    1.67    16.04   True
Rv1780  -   hypothetical protein Rv1780     13  129.62  2.43    9.65    27.63   -5.22   0.79    6.00    1.85    10.46   True
Rv3821  -   PROBABLE CONSERVED INTEGRAL MEMBRANE PROTEIN    18  43.56   7.89    21.25   377.20  -2.28   3.69    5.97    1.66    19.45   True
Rv2632c -   hypothetical protein Rv2632c    3   21.33   0.00    7.41    44.97   -3.64   1.77    5.42    1.04    17.46   True
Rv3823c mmpL8   PROBABLE CONSERVED INTEGRAL MEMBRANE TRANSPORT PROTEIN MMPL8    78  11.63   2.53    4.70    51.32   -2.38   2.97    5.35    1.59    9.40    True
</pre>

Header/Comments are proceded with "#" tags, and are followed by a row for each ORF found in the input file. Below, each of the columns in is defined:  

<table>

<tbody>

<tr>

<th width="20%" align="left">Column Header</th>

<th width="80%" align="left">Column Definition</th>

<th></th>

</tr>

<tr>

<td>ORF</td>

<td>ORF ID found in annotation.</td>

</tr>

<tr>

<td>Name</td>

<td>Name of the gene found in the annotation.</td>

</tr>

<tr>

<td>Description</td>

<td>Description of the gene found in the annotation.</td>

</tr>

<tr>

<td>N</td>

<td>Total Number of TA dinucleotides within the ORF.</td>

</tr>

<tr>

<td>Mean WT-1</td>

<td>Mean read-count for reference strain in condition 1</td>

</tr>

<tr>

<td>Mean WT-2</td>

<td>Mean read-count for reference strain in condition 2</td>

</tr>

<tr>

<td>Mean KO-1</td>

<td>Mean read-count for knockout strain in condition 1</td>

</tr>

<tr>

<td>Mean KO-2</td>

<td>Mean read-count for knockout strain in condition 2</td>

</tr>

<tr>

<td>Mean logFC WT-2/WT-1</td>

<td>Average logFC for reference strain between condition 2 and 1</td>

</tr>

<tr>

<td>Mean logFC KO-2/KO-1</td>

<td>Average logFC for knockout strain between condition 2 and 1</td>

</tr>

<tr>

<td>Mean delta logFC</td>

<td>Average difference between the logFCs</td>

</tr>

<tr>

<td>L. Bound</td>

<td>Lower Bound of the 95% Highest Density Interval</td>

</tr>

<tr>

<td>U. Bound</td>

<td>Upper Bound of the 95% Highest Density Interval</td>

</tr>

<tr>

<td>Outside of HDI?</td>

<td>True if HDI is significanlty different than ROPE (i.e Significantly different logFCs, indicating an interaction)</td>

</tr>

</tbody>

</table>

#### <a name="exec">Running Time</a>

The execution of the software on the representative dataset takes roughly 10 minutes on running on a linux server using the default values. In general, this will depend on the number of genes in the genome being analyzed, the number of samples desired, and the number of replicate datasets. The number of samples to be taken can be controlled using the flag "-s".

#### License
<center>  
GNU General Public License v3.0
© Copyright 2012. Michael A. DeJesus & Thomas R. Ioerger.</center>
