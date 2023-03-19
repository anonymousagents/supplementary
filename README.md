Our main focus is on addressing the issue of noisy rewards. Therefore, in our submission, we did not elaborate on the **requirements for function $g$** in great detail. However, we agree with the reviewer that a more **detailed discussion on the choice of $g$** would be beneficial and we plan to include it in the revised version of the paper. To this end, we investigated a selection of candidates for $g$ on the task of hard attention for digit recognition (cf. Sec. 3 and Fig. 3a) using the RAM model (cf. Sec. 5.1). In addition to the logarithmic and linear function, we conducted experiments on $g(x) = e^x$ and $g(x) = x^{\alpha}$, where $\alpha = 1/6, 1/3, 3, 6$, with reward hacking (to examine its information fidelity) and without reward hacking (to examine its performance in joint learning of the policy and the discriminator).

$\diamond$ [Figure A. Plot of rewards when using different $g$ functions. The x-axis is the posterior probability $q_\phi(y|\tau) \in [0,1]$, and the y-axis is the reward value calculated according to Eq. 9 with reward clipping](https://github.com/anonymousagents/supplementary/blob/main/resources/figure1-g-functions.jpg)

The key points are presented below.



<!-- **1.1) Analysis of different candidates for $g$** -->




**1.2) Information Transmission**
As is stated in the submission (lines 244--246 right column; 293--295 right column), **$g$ should be chosen to preserve the information transmission ability** (from an internal discriminator to the policy network) such that maximizing the surrogate reward also leads to the maximization of mutual information $I(y;\tau)$. Measurement of information remains an open question and varies from task to task [1, 2]. A good $g$ function should at least work well where the noise is absent or low. 

Fortunately, with **reward hacking** where we suppress the noise with a fully trained discriminator, we can measure the information fidelity of a function $g$ by its performance difference from the logarithmic function (Note that to maximize $I(y;\tau)$, the $\log$ function works ideally in theory where noise is not an issue). 

<!-- Here, we require that $g$ should be 1) able to preserve information and 2) less sensitive to noise. We plot the mentioned candidates to further analyze them from these two perspectives.



- Their curvation similarity to the logarithm can help understand the diverse performance from an information transmission perspective. Please further check Analysis 1 for details.
- On the other hand, as is analyzed in Eq. 14 (line 263, right column) in the paper, the impact of noise (variance) can be analyzed by examining the first- (slope) and second-order derivative (curvature). Please further check Analysis 2.

-->


$\diamond$ [Figure B. Information fidelity **with** reward hacking experiments for $g$ functions.](https://github.com/anonymousagents/supplementary/blob/main/resources/figure2-reward-hacking-all.jpg)

<!-- In reward hacking experiments, the noise is regarded as sufficiently low since the rewards are provided by an approximated oracle discriminator. The discriminator is a control variable such that $g$’s ability to preserve information can be reflected by its performance difference from the logarithmic function.  -->
- **Analysis 1**: We note that to maximize $I(y;\tau)$, the $\log$ function works well where reward noise is deemed low, e.g., in our reward hacking setup. As illustrated in Fig. 7 of the paper, where the $\log$ and linear $g$ functions are tested, and in Fig.  of the supplementary material (see the link above), where more $g$ functions are tested, the linear function exhibits comparable performance to the logarithmic function as a baseline, whereas other functions result in varying degrees of performance degradation [#], among which $x^6$ shows an unacceptable information loss. It can also be predicted and explained by Fig. A, where $x^6$ compress a wide range of values, say [0, 0.5], to values close to zero, leading to a dramatic ignorance of information in various observations.

**1.3) Noise Impact**

$\diamond$ [Figure C. The impact of noise quantified with and **without** reward hacking for individual $g$ function.](https://github.com/anonymousagents/supplementary/blob/main/resources/figure-3-noise-impact.jpg)
- **Analysis 2**: As is shown in the sub-figure (a) of Fig. C, the logarithm is sensitive to noise due to its high variance as we discussed in Sec. 4 *Reward Noise Moderation*, while the linear function shows a smaller gap between the trials with and without reward hacking, being more robust to noise. Though there are other $g$ functions that can be more tolerant of noise, e.g., $x^2$, $x^3$, $x^6$ present sufficiently lower $\mathbb{V}(\varepsilon)$ (being consistent in both theory and experimental results with smaller gap between solid and dashed lines in sub-figure (b) ), their upper-performance bounds are limited by their inefficiency in information transmission (see [#] in Analysis 1 or the values of accuracy reached in Fig. C), resulting in worse performance than the linear form. 

**1.4) Summary**
- When selecting a $g$ function, both its ability to **transmit information** and its capacity to **moderate noise** should be taken into consideration as they are both *necessary* conditions. 
- Linear function performs well in transimitting information as logorithm does, while being more noise-tolerant. 
- Our experimental results provide strong evidence that the proposed linear function is effective, which reinforces the technical contributions of our work. 
