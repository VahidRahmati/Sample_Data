The text is under process (draft)


---
"CaBBI: calcium imaging analysis using biophysical models and Bayesian inference"
---
* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

introduction text

# Calcium imaging technique

Calcium imaging technique have been used to monitor the dynamic activity of both individual neurons and neuronal populations. The data acquired by this technique are fluorescence traces which encode the change in (intracellular) calcium concentration ([Ca2+]). The firing activities are encoded as fluorescence transients comprised of a rising phase and a decaying phase. In brief, when a neuron fires, [Ca2+] elevates (rising phase) due to the opening of voltage-gated calcium channels (mainly L-type channels), which allows for Ca2+ influx to the cytoplasm. This initial increase is then decayed exponentially by removal mechanisms such as buffering (decaying phase).           
The accurate reconstruction of firing activity (like spike trains) from fluorescence traces is restricted because of e.g. noise, the low temporal resolution (in regular configurations), as well as by an indirect nonlinear relationship between [Ca2+] kinetics and the membrane potential.

## CaBBI
We proposed a new method called CaBBI (Rahmati et al. 2016): calcium imaging analysis using biophysical models and Bayesian inference. This method has several important advantages over previously introduced methods, like template matching methods or machine learning methods. The main advantages are as follows. First, CaBBI incorporates biophysical mechanisms of spike and/or burst generation, by using the spiking and bursting neuron models. Using such priors allows for a much more reliable inference about firing events. Second, it can intrinsically be adapted to fluorescence transients with different rise and/or decay times. In particular, it can even reconstruct the firing events from relatively slowly rising fluorescence transients, which are typical for recordings with new genetically-encoded calcium indicators. Third, CaBBI not only reconstruct the firing patterns, but also estimates the biophysical quantities such as membrane potential, [Ca2+] kinetics, and voltage-gated currents/conductances. For more details see (Ramati et al. 2016).  

CaBBI uses biophysical generative models of fluorescence data. The generative model is composed of evolution and observations equations. The evolution equation contains the differential equations of the spiking model, calcium channel, [Ca2+] kinetics. The generated [Ca2+] kinetics are then mapped nonlinearly to the fluorescence kinetics through the  observation equation. For a given fluoresce trace, the task is to infer the quantities of interest of evolution equations causing this trace; e.g. the underlying membrane potential (thus, spikes/burst), or [Ca2+] kinetics. For inversion, CaBBI uses a well-established variational Bayesian method implemented in the VBA-toolbox [citation].

<!-- insert an image -->
![]({{ site.baseurl }}/images/wiki/CaBBI/Fig 1.png)

> **CaBBI**
Graph illustrating CaBBI's generative model and its inversion, which are comprised of evolution (i.e., a neuron model) and observation equations. The represented hierarchy in the graph displays how neuronal dynamics relate to fluorescence traces.

As two examples of spiking models, we used the well-known Fitzhugh-Nagumo (FHN) model and the Quadratic-Gaussian Integrate-and-Fire model (QGIF). The QGIF model is a new I&F model which does not require any reset condition neither for producing the rising phase of the spike nor for its re-polarization phase (Rahmati et al. 2016).    

### CaBBI in *two* steps (for **new data**)
Currently, we have provided demo_CaIBB_FHN and demo_CaIBB_QGIF which invert the FHN and QGIF generative models, respectively, for the given fluorescence trace. Each script has 11 Steps (parts); see the next section for more details. However, to quickly apply CaBBI to your calcium imaging data, you just need to simply: i) determine the directory to your fluorescence trace (as a vector) in Step. 1,, and ii) determine the sampling rate (in [ms]) used to record this trace in Step. 2. The code will   


#### CaBBI demo codes
Currently, we have provided demo_CaIBB_FHN and demo_CaIBB_QGIF which invert the FHN and QGIF generative models, respectively, for the given fluorescence trace. As sample fluorescence traces, these codes automatically download and then invert the traces published by CaBBI [link].


These *in-vitro* data were recorded from neonatal hippocampal tissues. In this age, the fluorescence transients have relatively very slow rising phases, which makes the inversions profoundly more difficult. The similar slowly rising transients have been observed for data recoded by means of the new genetically-encoded calcium indicators.
Each of these demo scripts has 11 steps (sections), where the actions of the main code-lines were commented in front of them. However, here we briefly review the these steps.







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
