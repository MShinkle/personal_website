---
title: "Understanding and Controlling the Brain with Feature Visualization"
date: 2025-06-11
image: /assets/img/feature-visualization/individual_voxels.png
tags: [Neuroscience, AI]
links:
  - name: CVPR Paper
    url: https://arxiv.org/abs/2501.01960
  - name: Interactive Viewer
    url: http://piecesofmind.psyc.unr.edu/pycortex/activation_maximization/
---

*This work was done while working as a PhD student at the University of Nevada, Reno, supervised by [Mark Lescroart](https://piecesofmind.psyc.unr.edu/).*

*If you only come away with one thing from this post, it should probably be this [interactive brain viewer](http://piecesofmind.psyc.unr.edu/pycortex/activation_maximization/). Click anywhere on the brain that is colored to see an image synthesized for that specific chunk of the brain!*

In my previous work on [Simulating the Human Brain with Neural Networks](/projects/mapping-dnn-visual-cortex/), I developed techniques for building a simulation of the human visual system using DNN-based encoding models. One thing that this enables is taking interpretability methods originally designed for understanding DNNs and applying them to the brain instead. That's what this project is about.

Feature visualization (also known as activation maximization) is a technique for visualizing what different parts inside a DNN optimally respond to. The idea is simple: start with a blank or noisy image, and iteratively modify it via gradient ascent to maximize the response of one output of a DNN. The resulting image can be thought of as revealing what that neuron is "looking for"--images optimized for the 'dog' output of a DNN tend to spam dog-like features everywhere:

![Feature visualization of a DNN's 'dog' output](/assets/img/controlling-brain-responses/deepdream_dog.jpg){: .two-thirds-width .rounded}

<div class="section-divider"></div>

### Applying Feature Visualization to the Brain

Thus far this technique has just been applied to DNNs. But because my brain simulations are built on top of a DNN, I can apply this same technique to a simulated brain. Instead of optimizing the image for the response of a DNN, I optimize for the simulated response of a brain region, or even a single cortical voxel!

Individual voxels are notoriously noisy. Yet images optimized for them often contain highly interpretable contents -- apparent eyes and mouths in voxels from face-processing regions, buildings and scene layouts in voxels from scene-processing regions, and body-like features in body-processing regions. This holds throughout many category-selective regions like these, but also applies to earlier visual areas, with images for voxels in areas like V1 and V2 containing high-contrast patches in the expected retinotopic visual positions.

![Individual voxel visualizations](/assets/img/feature-visualization/individual_voxels.png){: .two-thirds-width}

*You can see selected examples of these above, but if you want to explore for yourself, check out this [interactive viewer](http://piecesofmind.psyc.unr.edu/pycortex/activation_maximization/)!*

In my opinion, exploring voxels *outside* of established regions is particularly interesting. For example, I found a cluster of voxels in posterior parietal cortex whose generated images contain scene-like imagery. At the time this was a novel observation; a scene-selective area in this location has since been [independently discovered by other researchers](https://elifesciences.org/reviewed-preprints/91601v2). Exploring along the posterior boundary of PPA reveals some voxels whose images contain upright body-like shapes -- if you find this interesting, check out our [recent work](/projects/novel-body-selective-regions/) finding body-selectivity in exactly this spot.

If you find anything interesting yourself, I'd love to hear about it!

<div class="section-divider"></div>

### Feature Visualization for *Controlling* Brain Responses

These images are optimized for simulated brain responses. But do they *actually* drive *real* responses in the human brain? We tested this by first generating images for five brain regions with diverse visual selectivity--V3 (early visual processing), LO (objects), EBA (bodies), FFA (faces), and RSC (scenes). We then collected real fMRI responses from human subjects while they viewed these synthesized images.

This actually works quite well! For both of our subjects, 4/5 region responded most strongly to images optimized specifically for them, over images optimized for other regions. 

![ROI-optimized images and results](/assets/img/feature-visualization/roi_images.png)

We also found in a second analysis (see the paper) that these images generalize well across subjects -- images optimized based on simulated responses for one set of subjects succeed at controlling the real brain responses in a *separate* set of subjects.

<div class="section-divider"></div>

### Pushing the Limits of This Method

I wanted to really push this method to its limits. To do this, I took pairs of brain regions with highly similar selectivity (e.g. both face-selective), and tried to optimize images that *specifically stimulated one, but not the other*. I also introduced an additional constraint: instead of generating images from scratch, I started with images that are already highly stimulating to both regions, and only allowed small-scale modifications to these images.

Just looking at the images themselves, the results are actually somewhat interpretable. Optimizing for OFA tends to pull out individual facial components like eyes and mouths, whereas optimizing for FFA tends to blur these features while retaining overall facial structure. This generally matches the different roles in face processing suggested in prior literature.

![Face region contrast results](/assets/img/controlling-brain-responses/face_contrasts.png)

We also wanted to see whether these images *actually* drove just the region they were optimized for, so we presented them to human participants and collected their real fMRI responses. This worked... somewhat? For Subject 1, images optimized for OFA drove OFA responses more strongly than did images optimized for FFA, and vice-versa for FFA. However, results for Subject 2 were less clear. A similar experiment using two scene-selective regions (OPA and PPA) also largely failed. But the fact that this worked at all, given that we were explicitly trying to push the method to its limits, makes me excited for what future improvements of these methods might enable.

<div class="section-divider"></div>

### Takeaways

This project shows that DNN-based simulations enable the use of techniques that would otherwise be impractical or impossible with the human brain. This is a powerful opportunity to study the brain like never before. DNN-to-brain mappings mean that many techniques from AI interpretability can be translated directly to neuroscience, meaning that as our understanding of AI grows, so can our understanding of the brain.

*For more details and results, see my CVPR 2025 paper [Visualizing and Controlling Cortical Responses Using Voxel-Weighted Activation Maximization](https://scholar.google.com/citations?view_op=view_citation&hl=en&user=da0OafkAAAAJ&sortby=pubdate&citation_for_view=da0OafkAAAAJ:ufrVoPGSRksC) and my [dissertation](https://search.proquest.com/openview/afad0c719d928c55d636640b71b28b94/1?pq-origsite=gscholar&cbl=18750&diss=y).*

*This work was also presented at CVPR 2025 and the Vision Sciences Society 2023 and 2024 Annual Meetings.*
