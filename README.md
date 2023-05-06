Download Link: https://assignmentchef.com/product/solved-ee2025-programming-assignment-2
<br>
You can use MATLAB, Python or any other tool for this programming assignment.

<strong>Objectives: </strong>In the previous assignment, you implemented a QAM modulator and demodulator and simulated the transmission of an image of Mona Lisa through an additive white Gaussian noise channel. In this assignment, we will improve the bit error rate (fraction of bits incorrectly decoded) using channel coding. We will see three different channel coding schemes, and the tradeoff between the transmission rate and the probability of error.

We will work with a larger image, which can be found as an attachment. Use the following code to load the image:

Python:

import numpy as np

from matplotlib import pyplot as plt MSS = np.load(‘mss.npy’) plt.imshow(MSS,‘gray’), plt.show()

MATLAB: load mss.mat imshow(mss, [0 1])

The image, in all, contains 400 × 300 = 120000 information bits.

<strong>Implementation: </strong>You will encode, modulate and transmit the image using 4-QAM modulation scheme that you implemented in Assignment 1 and the channel codes described below. Use the same parameters for <em>T,f<sub>c</sub>,f<sub>s </sub></em>as earlier.

First, simulate the communication for the following values of the noise variance in the discrete-time model: <em>σ</em><sup>2 </sup>= 20<em>,</em>12<em>,</em>7<em>,</em>5. This is an unfair comparison, since the total energy used to transmit the image is different for each code (because of the difference in rates). Next, simulate the communication for 5 values of E<em><sub>b</sub>/N<sub>o</sub></em>: −2<em>,</em>0<em>,</em>2<em>,</em>4<em>,</em>6 dB. Since E<em><sub>b </sub></em>is the energy <em>per message bit, </em>the corresponding noise variance depends on the rate of the code.

For each of these scenarios, you will plot the received image (which will be noisy) and report the number of pixels that were wrongly decoded.

Also plot the bit error rate, or BER (fraction of pixels that were wrongly decoded) versus <em>E<sub>b</sub>/N</em><sub>0</sub>. For plotting the BER, use a logarithmic <em>y</em>-axis, i.e., use plt.semilogy instead of plt.plot in Python and semilogy instead of plot in MATLAB.

These four figures, the corresponding number of wrong pixels, and the plot must be reported in a single pdf file. Attach your code in a separate .py/.m/.ipynb file.

<strong>Blocks in the digital communication system</strong>

The input to the digital communication system is a binary image (a matrix) of size 400 × 300. This can be equivalently represented as a <em>n<sub>m </sub></em>= 120000 length binary vector <em>m</em><sub>1</sub><em>,…,m<sub>n</sub></em><em><sub>m</sub></em>. The transmitter has two blocks:

Figure 1: Block diagram of the digital communication system. We communicate an image file using an error correcting code and 4-QAM modulation scheme.

<ol>

 <li>Channel encoder: This block takes <em>k </em>information bits at a time, and outputs <em>n </em>codeword bits. The codeword bits are all concatenated and sent to the next stage. For <em>i </em>= 0<em>,</em>1<em>,…,n<sub>m</sub>/k </em>− 1, the encoder takes <em>m<sub>ik</sub>,…,m</em><sub>(<em>i</em>+1)<em>k</em>−1 </sub>and outputs <em>c<sub>in</sub>,…,c</em><sub>(<em>i</em>+1)<em>n</em>−1</sub>. We will describe this block in more detail later. The output of this block will be a binary vector of length <em>n<sub>ch </sub></em>= <em>n<sub>m </sub></em>× <em>n/k </em>= 120000 × <em>n/k</em>. Call this</li>

</ol>

<em>c</em>0<em>,…,c</em><em>n</em><em>ch</em>−1.

<ol start="2">

 <li>Modulator: This block takes 2 codeword bits at a time, <em>c</em><sub>2<em>i</em></sub><em>,c</em><sub>2<em>i</em>+1 </sub>and outputs a continuous signal of duration <em>T</em>. In the discrete-time model, the modulator takes <em>c</em><sub>2<em>i</em></sub><em>,c</em><sub>2<em>i</em>+1 </sub>and outputs a vector of length <em>Tf<sub>s </sub></em>= 50 (in our case) with real-valued entries. Use the 4-QAM modulator from Assignment 1.</li>

</ol>

The output of this block will be a real-valued vector of length <em>Tf<sub>s </sub></em>× <em>n<sub>m </sub></em>× <em>n/</em>(2<em>k</em>) = 25 × 120000 × <em>n/k</em>.

The transmitted signal is corrupted by additive white Gaussian noise with the appropriate variance, as in the previous assignment. It is important to note that E<em><sub>b</sub>/N</em><sub>0 </sub>depends on the rate of the channel code. In particular, E<em><sub>b </sub></em>is equal to <em>n/k</em>× energy of one coded binary symbol. Hence, you will need to recompute the noise variance for each channel code.

The decoder has two stages:

<ol>

 <li>Demodulator: In the discrete-time channel model, the demodulator takes <em>Tf<sub>s </sub></em>samples at a time and outputs 2 codeword bits. All the codeword bits are concatenated and sent to the next stage. Use the demodulator from Assignment 1.</li>

</ol>

The output of this block will be a binary vector of length <em>n<sub>ch </sub></em>= 120000 × <em>n/k</em>. Call this ˆ<em>c</em><sub>0</sub><em>,…,c</em>ˆ<em><sub>n</sub></em><em><sub>ch</sub></em>.

<ol start="2">

 <li>Channel decoder: This takes <em>n </em>demodulated bits at a time, say ˆ<em>c<sub>in</sub>,…,c</em>ˆ<sub>(<em>i</em>+1)<em>n</em>−1 </sub>and decodes each to <em>k </em>message/information bits ˆ<em>m<sub>ik</sub>,…,m</em>ˆ<sub>(<em>i</em>+1)<em>k</em>−1</sub>. We describe this in more detail later.</li>

</ol>

The output of this block should be the decoded image, which is a binary matrix of size 400 × 300.

<strong>Channel coding and decoding</strong>

Recall that a channel encoder <em>f </em>is a function that takes <em>k </em>information/message bits as input, and outputs a codeword of <em>n </em>bits. The decoder <em>g </em>takes <em>n </em>received bits as input, and outputs <em>k </em>bits that correspond to an estimate of the message.

We will use three different channel codes:

<ol>

 <li>A rate 1<em>/</em>2 linear code with <em>n </em>= 8 and <em>k </em>= 4. The relationship between the <em>k </em>information bit vector <em>m<sup>k </sup></em>and <em>n</em>-length codeword vector <em>x<sup>n </sup></em>is given by</li>

</ol>

where denotes transpose of matrix <em>G</em><sub>1 </sub>and

The decoder is the standard minimum Hamming distance decoder: Given a received vector <em>y<sup>n </sup></em>∈{0<em>,</em>1}<em><sup>n</sup></em>, the decoder outputs

<em>,</em>

where <em>d<sub>H </sub></em>is the Hamming distance between two vectors (i.e., the number of locations where the two vectors differ). For example, if <em>a </em>= (1<em>,</em>1<em>,</em>1), <em>b </em>= (0<em>,</em>1<em>,</em>1) and <em>c </em>= (1<em>,</em>0<em>,</em>0), then <em>d<sub>H</sub></em>(<em>a,b</em>) = 1 while <em>d<sub>H</sub></em>(<em>b,c</em>) = 3.

<ol start="2">

 <li>A rate 1<em>/</em>3 repetition code: The encoder takes 1 bit <em>m </em>as input and outputs 3 bits (<em>m,m,m</em>), i.e., repeats the message bit 3 times.</li>

</ol>

The decoder simply outputs the majority. Given <em>y</em><sup>3 </sup>= (<em>y</em><sub>1</sub><em>,y</em><sub>2</sub><em>,y</em><sub>3</sub>), it outputs ˆ<em>m </em>= 0 if there are at least 2 zeros in <em>y</em><sup>3</sup>, and outputs 1 otherwise.

<ol start="3">

 <li>A rate 1<em>/</em>3 linear code with <em>n </em>= 12 and <em>k </em>= 4: The encoder and decoder operate in a fashion similar to code 1, but instead of <em>G</em><sub>1 </sub>we take</li>

</ol>

<sup></sup>1    0    0    0    0    1    1    1    1    0    1    0<sup></sup>

<em>G</em>2 = <sub></sub>00 10  01  00  11  01  11  01  01  11  11 01<sub></sub>

0    0    0    1    0    0    0    1    1    1    1    1

The decoder is the minimum Hamming distance decoder for this code.

<strong>Remarks </strong>• Channel encoding and decoding is a computationally intensive process. The entire simulation should take about 25 − 30 minutes on a modern processor (like a core i3-i5). In particular, minimum Hamming distance decoding takes the most resources since you have to compute 2<em><sup>k </sup></em>distances every time you want to decode.

<ul>

 <li>In practice (e.g., cellular communication), we do not use the minimum distance decoder but instead carefully designed low-complexity decoders. It is remarkable how well modern codes perform: even with <em>k </em>in the thousands, our cell phones equipped with less powerful processors perform encoding and decoding quickly, and the bit error rates are far better than what you obtain here for the same rate and <em>E<sub>b</sub>/N</em><sub>0</sub>.</li>

</ul>

To give some perspective, codes with <em>n </em>= 1248, <em>k </em>= 624, and achieving BER 10<sup>−5 </sup>for <em>E<sub>b</sub>/N</em><sub>0 </sub>of 2<em>.</em>5dB with decoding time less than a millisecond are being used in practice today.

<ul>

 <li>When testing and debugging your code, you can instead use the Mona Lisa image from the previous assignment, or generate 10<sup>4 </sup>random message bits to improve speed.</li>

 <li>For the image provided in this assignment, when you plot the BER for different noise variances, you should observe that the bit error rate (BER) for code 1 is lower than uncoded transmission. This code has rate 1<em>/</em> The rate 1<em>/</em>3 repetition code further reduces the BER at the cost of lower transmission rate. Code 3 has even lower BER, but at the same rate 1<em>/</em>3. However, the price that we pay is increased complexity and delay.</li>

</ul>

You will notice that the ordering is different when you simulate for different E<em><sub>b</sub>/</em>