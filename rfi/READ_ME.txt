SKA RFI Challenge
Completed by Christopher Schollar ctgschollar@gmail.com

INSTRUCTIONS
My program runs on python 2.7 and uses the following libraries:
numpy
matplotlib
time
argparse
sys

As a first run I suggest running python ska_rfi_challenge.py --plot_data

EXPLANATION
The synthetic data I have is simply a normally distributed 2D array with a mean of 0 and standard deviation of 1 created using numpy. The x dimension represents frequency and the y Dimension represents time (in my plots).

I then randomly add constant wave tones of random length on random channels. The tones all have a power which is a multiple of the standard deviation (1). This multiple is randomly selected from a range of possible values (defualt 1-10)

After this I run a frequency domain Median Absolute Distance (MAD) Estimator to flag values which are greater than 6 sigma from a local median. The median of a window around the data point of interest is the used to calculate this local median, the default window is 10 values on either side of our data point. Although the algorithm is tested on a chunk of the data, the estimator can be run on a stream of data. As this is a window based algorithm, all RFI which within 1 window of the edge of our data cannot be detected.

You can also run the MAD estimator in TD mode, however with this data it does not get good results. The algorithm will only flag signals which are shorter than or equal to the window length and most of the randomly generated tones are longer than this. To see the effectiveness of the TD flagging simply run with the flag --with_TD

Finally I calculate the values of : 
total number of data points with known (inserted) RFI
total number of data points with no RFI
total number of false negatives
total number of false positives
total number of true positives
total number of true negatives
false positive / false negative 
true positive / false positive
true negative / false negative
Percentage of RFI flagged
Percentage of erroneously flagged data
The percentage of RFI in each power level flagged 

I also display the data and RFI maps if run with the --plot_data flag

OPTIONS
I have included a number of command line options to allow you to see the effect of changing parameters a description of each option follows (values in square brackets are the type of value expected for each option:
-w [int] or --window	This defines the length of the window used to calculate the MAD estimator. A longer window means better accuracy and more processing and vica versa
-s [int] or --sigma		This defines the threshold at which a value will be flagged as RFI, a lower value means more false positives and less false negatives and vica versa
-x [int] or --xDim 		This defines the number of frequency channels
-y [int] or --yDim 		This defines the number of times
-n [int] or --nRFI 		This defines the number of randomly inserted RFI tones
-l [int] or --max_level This defines the highest multiple of stdDev for the power of random tones
-m [int] or --min_level This defines the lowest multiple of stdDev for the power of random tones
--display_levels		DEFAULT When run with this flag I calculate and display the percentage of RFI detected for each power level
--no-display_levels		Do not calculate percentage of RFI detected for each power level
--plot_data				Plot data with matplotlin
--no-plot_data			DEFAULT do not plot data with matplotlib
--with_TD				Run RFI detection in Time Domain as well as Frequency Domain
--without_TD			DEFAULT do not run RFI detection in Time Domain