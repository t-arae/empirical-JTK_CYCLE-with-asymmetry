#Empirical JTK_CYCLE

Hi!

This is the README for running the rhythm detection method described in [Hutchison AL, Maienscein-Cline M, Chiang AH, Tabei SMA, Gudjonson H, Bahroos N, Allada R, Dinner AR. “Improved statistical methods enable greater sensitivity in rhythm detection for genome-wide data.” PLoS Computational Biology 2015 Mar. Vol. 11, No. 3, pp. e1004094. doi:10.1371/journal.pcbi.1004094](http://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1004094)

It is based on the original JTK_CYCLE code from [Hughes ME, Hogenesch JB, Kornacker K. JTK_CYCLE: an efficient nonparametric algorithm for detecting rhythmic components in genome-scale data sets. J Biol Rhythms. 2010 Oct;25(5):372-80. doi:10.1177/0748730410379711.](http://jbr.sagepub.com/content/25/5/372)

A previous version of this method empirically calculated the p-values using many permutations. We have recently sped up this calculation by approximating the null tau distribution using a Gamma distribution based on 1000 permutations. P-values can be derived from this model, allowing for p-values independent of the number of permutations. The description of this code can be found in [Hutchison AL _et al._ (2016), "BooteJTK: Improved Rhythm Detection via Bootstrapping"](http://www.biorxiv.org/content/early/2017/03/20/118521), available at bioRxiv.


##Applications:

An application of the method to mouse and human pancreas and liver can be found in [Perelis et al. "Pancreatic β cell enhancers regulate rhythmic transcription of genes controlling insulin secretion" (2015). 350(6261). aac4250. doi:10.1126/science.aac4250](http://science.sciencemag.org/content/350/6261/aac4250)

An application of the method to Drosophila neurons can be found in [Flourakis M et al. "A conserved bicycle model for circadian clock control of membrane excitability" Cell (2015). 162(4). 836-848. doi:10.1016/j.cell.2015.07.036](http://www.sciencedirect.com/science/article/pii/S0092867415009137)

An application of the method to 16S gut microbiome data can be found in [Leone VA et al. “Effects of diurnal variation of gut microbes and high fat feeding on host circadian clock function and metabolism” Cell Host-Microbe (2015). 17(5). 681-689. 13 May doi:10.1016/j.chom.2015.03.006](http://www.sciencedirect.com/science/article/pii/S1931312815001237)



## Execution

Use
<pre><code>chmod 755 eJTK-CalcP.py</code></pre>

to make eJTK-CalcP.py executable.

Type

<pre><code>cd bin/</code></pre>
<pre><code>python setup.py build_ext --inplace</code></pre>
<pre><code>cd ../bin/</code></pre>

to compile the Cython code.

### Running this command will procude three files



<pre><code>./eJTK-CalcP.py -f example/TestInput4.txt -w ref_files/waveform_cosine.txt -p ref_files/period24.txt -s ref_files/phases_00-22_by2.txt -a ref_files/asymmetries_02-22_by2.txt -x cos24_ph00-22_by2_a02-22_by2_OTHERTEXT</code></pre>



1. **example/TestInput4_cos24_ph00-22_by2_a02-22_by2_OTHERTEXT_jtkout.txt**
   This is the output of eJTK.py, it contains the best reference waveform matching each time series. Best is defined as having the highest Tau value. This becomes input for CalcP.py.


2. **example/TestInput4_cos24_ph00-22_by2_a02-22_by2_OTHERTEXT_jtknull1000.txt**
   This is the output of eJTK.py unless otherwise specified by the -n flag (see eJTK-CalcP.py -h for more information). It similar to *jtkout.txt only it contains the results of 1000 runs of Gaussian noise. It is also an input for CalcP.py


3. **example/TestInput4_cos24_ph00-22_by2_a02-22_by2_OTHERTEXT_jtkout_GammaP.txt**
   This is the output of CalcP.py. It is the equivalent of *jtkout.txt, only now with correct p-values as estimated by fitting the time series to a Gamma distribution. It also contains a column of these p-values adjusted with the Benjamini-Hochberg correction.

If you run the above command as is will produce files with a '_1' appended, as these files already exist in the examples folder.


## Output information:

The output *jtkout.txt and *GammaP.txt files have columns as described below

**ID**: Name of time series analyzed

**Waveform**: Waveform used in analysis

**Period**: Period of best matching reference waveform

**Phase**: Phase of best matching reference waveform

**Nadir**: Trough of best matching reference waveform

**Mean**: Mean of time series

**Std_Dev**: Standard Deviation of time series

**MaxLoc**: Location of maximum of time series

**MinLoc**: Location of minimum of time series

**Max**: Maximum value of time series

**Min**: Minimum value of time series

**Max_Amp**: Max-Min of time series

**FC**: Fold Change (Max/Min)

**IQR_FC**: Fold Chnage of 25% and 75% percentiles of time series

**Tau**: Highest Kendall Tau Correlation between time series and reference waveforms

**P**: P-value corresponding to Tau, uncorrected for multiple hypothesis testing

**BF**: Bonferroni adjusted P-value from P

**empP**: min(P-value calculated from empirical null distribution,BF)

**GammaP**: min(P-value calculated from Gamma Fit of empirical null distribution,empP)

**GammaBH**: Benjmanini-Hochberg adjusted p-value of GammaP


##Version information:

* Python 3.10.6
* gcc (Ubuntu 11.3.0-1ubuntu1~22.04.1) 11.3.0
* Cython          0.29.35
* numpy           1.25.0
* packaging       23.1
* pandas          2.0.2
* patsy           0.5.3
* pip             22.0.2
* python-dateutil 2.8.2
* pytz            2023.3
* scipy           1.10.1
* setuptools      59.6.0
* six             1.16.0
* statsmodels     0.14.0
* tzdata          2023.3
* wheel           0.37.1

## License:
This code is released with the MIT License. See the License.txt file for more information.
