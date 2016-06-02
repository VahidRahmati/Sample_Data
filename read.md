---
title: "CaBBI: calcium imaging analysis using biophysical models and Bayesian inference"
---
* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

In the following we describe the  

# Calcium imaging technique

Calcium imaging techniques have been used to monitor the neuronal activity of both individual neurons and neuronal populations. The data acquired by this technique are fluorescence traces which encode the change in (intracellular) calcium concentration ([Ca2+]). The firing activities are encoded as fluorescence transients, comprised of a rising phase and a decaying phase. In brief, when a neuron fires, [Ca2+] elevates (rising phase) due to the opening of voltage-gated calcium channels (mainly L-type channels), which allows for Ca2+ influx to the cytoplasm. This initial increase is then decayed exponentially by removal mechanisms such as pumping and buffering (decaying phase).           
The accurate reconstruction of firing activity (like spike trains) from fluorescence traces is restricted because of e.g. noise, the low temporal resolution (in regular recording configurations), as well as by an indirect nonlinear relationship between [Ca2+] kinetics and the membrane potential.

## CaBBI
We proposed a new method called CaBBI ([Rahmati et al. 2016][1]): *c* alcium imaging *a* nalysis using *b* iophysical models and *B* ayesian inference. This method has several remarkable advantages over previously introduced methods, including the template matching and machine learning methods. The main advantages are as follows. First, CaBBI incorporates biophysical mechanisms of spike and/or burst generation, by using the spiking and bursting neuron models. Using such priors allows for a much more reliable inference about firing events. Second, it can intrinsically be adapted to fluorescence transients with different rise and/or decay times. In particular, it can even reconstruct the firing events from relatively slowly rising fluorescence transients, which are typical for recordings with new genetically-encoded calcium indicators. Third, CaBBI not only reconstruct the firing patterns, but also estimates the biophysical quantities such as membrane potential, [Ca2+] kinetics, and some of voltage-gated currents/conductances. For more details, see ([Rahmati et al. 2016][1]).  

CaBBI uses biophysical generative models of fluorescence data. The generative model is composed of evolution and observation equations. The evolution equation contains the differential equations governing the dynamics of the spiking model, calcium channel, and [Ca2+] kinetics. The generated [Ca2+] kinetics are then mapped nonlinearly to the fluorescence kinetics through the  observation equation. For a given fluoresce trace, the task is to infer the quantities of interest causing this trace; e.g. the underlying membrane potential (thus, spikes/burst), [Ca2+] kinetics, and etc. For inversion, CaBBI uses a well-established variational Bayesian method implemented in the VBA-toolbox ([Daunizeau et al. 2014][2]).

<!-- insert an image -->
![](D:\Dropbox\Project\PAPER\draft\CodeForVBA\WIKI\Fig 1.png)

> **Figure 1. CaBBI**
Graph illustrating CaBBI's generative model and its inversion, which are comprised of evolution (i.e., a neuron model) and observation equations. The represented hierarchy in the graph displays how neuronal dynamics relate to fluorescence traces; adapted from ([Rahmati et al. 2016][1]).

As two examples of spiking models, we used the well-known Fitzhugh-Nagumo (FHN) model and the Quadratic-Gaussian Integrate-and-Fire model (QGIF). The QGIF model is a new I&F model which does not require any reset condition neither for producing the upstroke phase of the spike nor for its re-polarization phase ([Rahmati et al. 2016][1]).    

### **Your new data**: CaBBI in ==two steps==
Currently, we have provided [demo_CaIBB_FHN]( ) and [demo_CaIBB_QGIF]( ) which invert the FHN and QGIF generative models, respectively, for the given fluorescence trace. You can select either of them to apply to your data. Each script has 11 Steps (parts); see the next section for more details. However, to quickly apply CaBBI to your calcium imaging data, you just need to select simply: i) in Step. 1, determine the directory to your fluorescence trace (as a vector), and ii) in Step. 2, determine the sampling rate (in [ms]) used to record this trace. Then, run the code.
The script will automatically remove the low frequency drifts from the given trace, and invert the generative model for this pre-processed trace. The main inferred parameters and variables will be stored in an array called [CaBBI]( ) (see Step. 11).

####  CaBBI demo codes
The [demo_CaIBB_FHN]( ) and [demo_CaIBB_QGIF]( ) invert the FHN and QGIF generative models, respectively, for the given fluorescence trace. As sample fluorescence traces, these codes automatically download and then invert the traces which have been originally published by CaBBI [](https://figshare.com/s/e524c1d214d411e5869c06ec4b8d1f61). These *in-vitro* data were recorded from neonatal hippocampal tissues. Note in this age, the fluorescence transients have relatively very slow rising phases, which makes the inversions effectively more difficult. The trace will be automatically pre-processed so that its slow temporal drifts are removed. The pre-processed trace is then used to invert the generative model; i.e. FHN model in [demo_CaIBB_FHN]( ), and QGIF model in [demo_CaIBB_QGIF]( ). After inversion, the inferred membrane potential, spiking event times, and [Ca2+] kinetics will be plotted in new figure:

<!-- insert an image -->
![](D:\Dropbox\Project\PAPER\draft\CodeForVBA\WIKI\Sampel_DemoFHN.png)

> **Figure 2. Sample results of the CaBBI**
Sample results of the [demo_CaIBB_FHN]( ). First row: The  pre-processed fluorescence trace, Second row: The inferred [Ca2+] kinetics, Third row: the inferred membrane potentials, Fourth row: the inferred spiking event times, obtained by thresholding the inferred membrane potentials. Note the inferred [Ca2+] kinetics (second row) and membrane potentials (third row) show the corresponding posterior means. The black dashed line is the spike (or event) detection threshold (see Step. 10).  

The main inferred quantities can be found in the structure array called [](CaBBI) (see Step); including the pre-processed trace, and the inferred membrane potentials and [Ca2+] kinetics. This array also contains the inferred spiking event times in both units of [ms] and [frame]. Note the inferred times in [ms] are based on the sampling rate of the recordings, which needs to be set in Step 2.   
Each of the [demo_CaIBB_FHN]( ) and [demo_CaIBB_QGIF]( ) scripts have 11 steps (parts), and each line is explained in a comment next to it. Nevertheless, here we briefly point to some hints:
+ The fluorescence trace is imported in Step 1, where you need to set the directory to its file.
+ You can determine the sampling rate (in units of [Hz]) used to recorded the fluorescence trace in Step 2. This parameter determines the temporal resolution of the recorded fluorescence trace and is important for the inversion, as well as the converting the inferred spiking times from units of [frame] to [ms] (see Step 11).
+ A polynomial de-trending method is provided in Step 4 to remove the low frequency drifts from the given fluorescence trace [Rahmati et al. 2016][1]). In this step, you can select the degree of the polynomial which will be fitted to the drifts; either 3 or 4.
+ The main parameters of the generative model and the priors can be set in Step 5 and 7. The internal parameters of the VBA-toolbox, including those for controlling the outputs which are displayed during inversion, can be set in Step 7.
+ The displayed inferred [Ca2+] kinetics (see Figure 2)have been already added by [50] nM as the baseline [Ca2+]. You can change this baseline in Step 9:  
```matlab
Calcium_basal = 50;         % in [nM], the basal [Ca2+] concentration  
```
+ A spike (or event) detection threshold is used to detect and extract the spiking event times from the inferred membrane potentials (see the dashed line in Figure 2; third row). If necessary, you can change this detection threshold in Step  10:
```matlab
Detection_threshold  = 0;         % the spike (or event) detection threshold
```
+ The refractory period for the inferred spiking events can be set in Step 10:
```matlab
if (count > 1) && ( (j-indices(count-1)) > ceil(6/dt) )  
```
by default it has been set to 6 ms. This means that after each detected spike event, all the threshold crossing fluctuations within this period will be discarded. Note such fluctuations can be due to the noise in the inferred membrane potentials.

+ The main results of the CaBBi are stored in the array [](CaBBI) in Step 11. Other inferred quantities can be found in the main structure array called [](posterior); see Step 8.     

 To prevent the small fluctuation on the inferred membrane potential from bing falsely detected as spiking events we reject all such fluctuations within the refractory period of the inferred spiking events. We any other threshold passing from negative to positive for the next 6 ms to prevent false spike detection from potential high frequency fluctuations in the inferred membrane potentials.


$$
\dot x = f(x,u,\phi)
$$

<!-- insert an image -->
![]({{ site.baseurl }}/images/wiki/CaBBI/an-image.png)

> **Image title**
The image legend here

<!-- insert an link -->
[see reference](http://www.sciencedirect.com/science/article/pii/S1053811915004231){:target="_blank"}

[began first section tutorial]({{ site.baseurl }}/wiki/CaBBI/#First-level-header)

<!-- make a list -->
*  _italic_
* __bold__
* `monotype`

<!-- display code -->

```matlab
% matlab comment
demo_CaIBB
```
[1] Rahmati V, Kirmse K, Marković D, Holthoff K, Kiebel SJ (2016) Inferring Neuronal Dynamics from Calcium Imaging Data Using Biophysical Models and Bayesian Inference. PLoS Comput Biol 12(2): e1004736. doi:10.1371/journal.pcbi.1004736 "Rahmati et al. 2016"
[2] J. Daunizeau, V. Adam, L. Rigoux (2014), VBA: a probabilistic treatment of nonlinear models for neurobiological and behavioural data. PLoS Comp Biol 10(1): e1003441. "Daunizeau et al. 2014"
