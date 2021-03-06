# iFAMS
iFAMS, or interactive Fourier Analysis for Mass Spectrometry, is the Prell lab's home built deconvolution algorithm for heterogenous mass populations

If you use this program for an article or presentation, please cite the following article:

Cleary, Sean P.; Thompson, Avery M.; Prell, James S. "Fourier Analysis Method for Analyzing Highly Congested Mass Spectra of Ion Populations with Repeated Subunits" Analytical Chemistry, 2016, 88, 6205-6213, doi: 10.1021/acs.analchem.6b01088.

Cleary, Sean P.,Li, Huilin, Bagal, Dhanashri Loo, Joseph A., Campuzano, Iain D. G., Prell, James S. "Extracting Charge and Mass Information from Highly Congested Mass Spectra Using Fourier-Domain Harmonics" Journal of the American Society of Mass Spectrometry, 2018, doi: 10.1007/s13361-018-2018-7.

# Manual Version

iFAMS Manual Run Instructions

This version of iFAMS contains no GUI, in case you would like to run the program from, say, a command line.  You will need python version 3 run this.

These three scripts will do everything EXCEPT calculate the charge states and sub-unit mass.  Reason being, this would be rather awkward to do without a graphical interface.  If you've worked with the GUI, the program chooses points in the graph to calculate these, so without the graph, it might be kind of awkward to keep running the program to eventually get what you want. You can calculate the charge states manually much in the same way you do a mass spectrum though, remembering that each peak in the Fourier Spectrum is a charge state divided by the sub-unit mass (z/sub-unit mass).

I've attached three different scripts, 1. A script that will Fourier Transform a mass spectrum 2. One that will Inverse Fourier transform the FT data to get the individual charge state distributions, and 3. One that will calculate the statistics on the individual charge state distributions.

1.	For the Fourier Transform script, there are three inputs.  

-i (input): This is required, which is the name of the data file that you wish to Fourier Transform

-d (domain, optional, default = “abs”) This is optional in case you want the real Fourier spectrum instead of the absolute (absolute is the default, so if this is what you want, you won’t have to input this). Type “real” for real data.

-p (plot, optional, default = “no”) This is optional if you want the data plotted.  The default is no, so you don’t need to type this in if you want it plotted elsewhere.  It plots in MatPlotlib.

To run this script, open a command prompt, and make sure that the data that is being FT’d is in the same directory as the script.  Change to the directory where the script/file is stored, and type the command, 

"python    iFAMS_Fourier_Transform.py   -i test.txt" 

This would be the command to Fourier Transform that test file included on my github page.  Important to note that the data needs to be a .txt file, and the m/z values need to be in the first column (abundance in the 2nd).  It will automatically export the Fourier Spectra as a CSV file.


2.	For the inverse Fourier Transform script, you'll need 3 inputs, but there are two optional values

-i (input) This is required, which is the name of the data file that you wish to do the full analysis on.  To be clear, this the file that hasn’t been Fourier transformed.  This is NOT the Fourier transformed one.

-sm (subunit mass) This is required, and it is the sub-unit mass.  

-cs (charge states) This is required, and it is the charge states you wish to use.  You’ll need to input with brackets around the full list, and use commas to separate the different charge states.

-d (domain, optional, default = “abs”) This is optional in case you want the real Fourier spectrum instead of the absolute (absolute is the default, so if this is what you want, you won’t have to input this). Type “real” for real data.

-p (plot, optional, default = “no”) This is optional if you want the data plotted.  The default is no, so you don’t need to type this in if you want it plotted elsewhere.  It plots in MatPlotlib.

 As an example, if you wanted to do the full analysis on the test file using a subunit mass of 678 and charge states 12-15+, you would type:

 "python      iFAMS_Inverse_Fourier_Transform.py    -i test.txt    -sm 678     -cs [12,13,14,15]"


3. For the statistical calculator script, you will need 4 arguments.  

-i (input)This is required, and it is the name of the IFFT file.  This should be a CSV file. Important to note, these should be the absolute value IFFT files, not the real ones

-cs (charge state) This is required, and it's the charge state of the IFFT file that you want to analyze

-sm (subunit mass) This is required, and it's the subunit mass that you wished to use.

-bm (base mass, optional) Often the subunit will be attached to some known mass, say lipids on a protein. The protein mass would be the basemass, so in other words, it's the section of the total mass that isn't changing.  The default is 0, so if there is no basemass, you don't have to enter anything here   

An example of this would be:

"python      iFAMS_statistical_calculator.py -i testIFFT12.csv      -cs 12      -bm 49320     -sm 678" 

this time, it will print the info in the command line. This script is a simple script that calcualtes the variance over all of the points, and takes the square root of that variance to produce the standard deviation.  It should be noted that if the distribution is narrow and doesn't occupy a large portion of the mass spectrum, it may be more accurate to fit a gaussian to the distribution rather than the entire spectrum.  I may update this script at some later point to allow for that.

4. For the zero-charge spectrum script, you will need 2 arguments.  You will also need the "IFFT" files from the inverse fourier transform script in the same folder.

-i (input) This is required, and it is the name of the original file that you fourier transformed.  It will look for the for the IFFT files based on the name of the file.

-cs (charge states) This is required, and it is the charge states that you want to contribute to the the zero-charge spectrum.  The input format is brackets with the charge states seperated by commas.  So for example, with the "test" file, the command would be

-f (optional, flip the data) I've included this incase x and y are in opposite columns.  Just type -f yes if this is the case.  default is no

"python iFAMS_zerocharge.py -i test.txt -cs [12,13,14,15]"

it will plot your spectrum using Matplotlib

If you have any questions, feel free to contact me, and I’ll be happy to help!!

# iFAMS GUI (iFAMS_V5.zip)

The newest version of the GUI can be found here! 
https://github.com/seanpatcleary/iFAMS/releases
This newest version includes the ability to Gabor transform spectra (read about it here! https://doi.org/10.1002/cphc.201900022) .
