**1) Discussion**

Our main focus is on addressing the issue of noisy rewards. Therefore, in our submission, we did not elaborate on the **requirements for function $g$** in great detail. However, we agree with the reviewer that a more **detailed discussion on the choice of $g$** would be beneficial and we plan to include it in the revised version of the paper. To this end, we investigated a selection of candidates for $g$ on the task of hard attention for digit recognition (cf. Sec. 3 and Fig. 3a) using the RAM model (cf. Sec. 5.1). In addition to the logarithmic and linear function, we conducted experiments on $g(x) = e^x$ and $g(x) = x^{\alpha}$, where $\alpha = 1/6, 1/3, 3, 6$, with reward hacking (to examine its information fidelity) and without reward hacking (to examine its performance in joint learning of the policy and the discriminator). We plot the mentioned candidates to further analyze them from these two perspectives. 

<img src="resources/figure1-g-functions.jpg" width="450">

$\diamond$ Figure A. Plot of rewards when using different $g$ functions. The x-axis is the posterior probability $q_\phi(y|\tau) \in [0,1]$, and the y-axis is the reward value calculated according to Eq. 9 with reward clipping.


The key points are presented below, where we require that $g$ should be 1) able to preserve information and 2) less sensitive to noise. 



**1.1) Information Transmission**

As is stated in the submission (lines 244--246 right column; 293--295 right column), **$g$ should be chosen to preserve the information transmission ability** (from an internal discriminator to the policy network) such that maximizing the surrogate reward also leads to the maximization of mutual information $I(y;\tau)$. Measurement of information remains an open question and varies from task to task [1, 2]. A good $g$ function should at least work well where the noise is absent or low. 

Fortunately, with **reward hacking** where we suppress the noise with a fully trained discriminator, we can measure the information fidelity of a function $g$ by its performance difference from the logarithmic function (Note that to maximize $I(y;\tau)$, the $\log$ function works ideally in theory where noise is not an issue). 


<img src="resources/figure2-reward-hacking-all.jpg" width="450">

$\diamond$ Figure B. Information fidelity **with** reward hacking experiments for $g$ functions.

As illustrated in Fig. B (see also Fig. 7 of the paper) the linear function exhibits comparable performance to the logarithmic function as a baseline, whereas other functions result in varying degrees of performance degradation, among which $x^6$ shows an unacceptable information loss. It can also be predicted and explained by Fig. A, where $x^6$ compress a wide range of values, say [0, 0.5], to values close to zero, leading to a dramatic ignorance of information in various observations.

**1.2) Noise Impact**

<img src="resources/figure-3-noise-impact.jpg" width="1000">

$\diamond$ Figure C. The impact of noise quantified with and **without** reward hacking for individual $g$ function.

As is shown in the sub-figure (a) of Fig. C, the logarithm is sensitive to noise due to its high variance as we discussed in Sec. 4 *Reward Noise Moderation*, while the linear function shows a smaller gap between the trials with and without reward hacking, being more robust to noise. Though there are other $g$ functions that can be more tolerant of noise, e.g., $x^2$, $x^3$, $x^6$ present sufficiently lower $\mathbb{V}(\varepsilon)$ (being consistent in both theory and experimental results with smaller gap between solid and dashed lines in sub-figure (b) ), their upper-performance bounds are limited by their inefficiency in information transmission, resulting in worse performance than the linear function. 

**1.3) Summary**

- When selecting a $g$ function, both its ability to **transmit information** and its capacity to **moderate noise** should be taken into consideration as they are both *necessary* conditions. 
- Linear function performs well in transimitting information as logorithm does, while being more noise-tolerant. 
- Our experimental results provide strong evidence that the proposed linear function is effective, which reinforces the technical contributions of our work. 

<!-- - Their curvation similarity to the logarithm can help understand the diverse performance from an information transmission perspective. Please further check Analysis 1 for details.
- On the other hand, as is analyzed in Eq. 14 (line 263, right column) in the paper, the impact of noise (variance) can be analyzed by examining the first- (slope) and second-order derivative (curvature). Please further check Analysis 2.
 -->
**2) Revisions**

This extensive discussion enhances our understanding of the fundamental role of $g$ regarding the IRRL joint learning process. To include this study in the revision, we will update the manuscript as below:
* after the existing sentence "where $g$ is an increasing function (e.g. $\log$), such that maximizing $g(·)$ leads to the maximization of the mutual information $I(y;\tau)$" in sec. 4.1 (lines 244-246 right column), a sentence "Thus, when selecting the appropriate $g$ function, it is important to consider both its ability to transmit information and its capacity to moderate noise. The former ensures that the maximization of mutual information can be achieved efficiently, while the latter helps to reduce the impact of reward noise." will be added; 
* and add an appendix section titled "Analysis of $g$ functions" which introduces the discussion we reported in this response. This appendix section will be referred to in sec. 5.4, as a supplementary material of Figure 7, which only involves the $\log$ and linear $g$ function. 

<!-- This additional study enhances the strength of the paper compared to the initial submission. Thank you to reviewer AqZR again for providing this valuable suggestion. -->

References:

[1] Nowozin, Sebastian, Botond Cseke, and Ryota Tomioka. "f-gan: Training generative neural samplers using variational divergence minimization." Advances in neural information processing systems 29 (2016).

[2] Ozair, Sherjil, Corey Lynch, Yoshua Bengio, Aaron Van den Oord, Sergey Levine, and Pierre Sermanet. "Wasserstein dependency measure for representation learning." Advances in Neural Information Processing Systems 32 (2019).
