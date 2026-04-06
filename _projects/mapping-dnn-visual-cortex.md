---
title: "Simulating the Human Brain with Neural Networks"
date: 2025-01-01
image: /assets/img/prf_brain.jpg
tags: [Neuroscience, AI]
links:
  - name: CVPR Paper
    url: https://arxiv.org/abs/2501.01960
---

*This work was done while working as a PhD student at the University of Nevada, Reno, supervised by [Mark Lescroart](https://piecesofmind.psyc.unr.edu/).*

Vision AI systems often learn internal representations that closely resemble features represented in the human brain. In this project, I developed a set of techniques for using DNN activations to simulate fMRI responses across the human visual cortex. I find that these methods are highly effective, enabling novel neuroscience experiments without any additional brain responses.

<div class="section-divider"></div>

### The Approach

My core goal was to build a predictive mapping from DNN activations to brain responses. For a DNN, I selected the Inception V3 model, a convolutional neural network trained for classifying the contents of images. I then took a large set of video clips, broke them into frames, and recorded the responses of each internal layer of the inception model to each frame of the videos. I then took actual human brain responses, recorded via fMRI, and used regression to learn a computational 'mapping' between DNN activations and brain responses.

![Methods overview](/assets/img/mapping-dnn-visual-cortex/methods.png){: .two-thirds-width}

A key challenge was managing the high dimensionality of DNN activations—even a relatively compact network generates millions of activations per stimulus. Prior work has addressed this by only using individual DNN layers or by applying dataset-specific dimensionality reduction. Both approaches have clear downsides: single-layer models can't capture the full range of cortical selectivity, and dataset-dependent reductions require additional data and recalibration for each new setting. I tried a variety of approaches to handling this, but ultimately found that a combination of adaptive spatial pooling and applying PCA to the *weights* of the Inception model was as or more effective at preserving brain-like features compared to previous approaches, while avoiding all of their downsides.

The end result of this is essentially a **simulation** of the visual system in the human brain--we can take new images or videos that neither the DNN or the human have ever seen, and simulate how their brain would respond.

<div class="section-divider"></div>

### Do the Simulations Actually Match the Real Brain?

I first wanted to measure how accurate our simulations were in the parts of the brain early in visual processing—the areas that handle things like edges, contours, and basic spatial patterns. I simulated responses to standard "retinotopy" stimuli—moving bars, rotating wedges, and expanding rings designed to map out which parts of the brain respond to which parts of the visual field—and analyzed them using the same pipeline we use to analyze real brain responses to the same stimuli. The resulting maps correspond closely with those from real brain data, including clear boundaries between early visual areas V1, V2, and V3.

![Retinotopic mapping comparison](/assets/img/mapping-dnn-visual-cortex/retinotopy.png)

I then wanted to see whether these simulations also work for areas later in visual processing—the regions that respond to higher-level things like faces, bodies, and scenes. I simulated responses to standard images from each of these categories and compared the resulting patterns to real brain data. The results closely match: body selectivity in EBA, face selectivity in OFA and FFA, scene selectivity in OPA, PPA, and RSC—all emerge clearly, highlighting the same regions that light up when real human subjects view these images.

![Category selectivity contrasts](/assets/img/mapping-dnn-visual-cortex/category_contrasts.png)

<div class="section-divider"></div>

### Why This Matters

The practical upside here is significant. Once the initial brain data is collected to learn the mapping, anyone can run their own simulated neuroscience experiments in hours or minutes, even on a mid-range computer. This makes neuroscience research vastly cheaper, faster, and more accessible. (For a follow-up using this approach to understand motion processing in scene-selective brain areas, see [DNN-Based Simulations of Motion Selectivity](/projects/dnn-motion-selectivity/).)

Another strength of this approach is that we can do things you *couldn't* do with real human subjects. You can intervene inside the DNN, such as disabling or stimulating one part, to simulate the effects of brain damage. Another example is taking interpretability methods originally designed to understand DNNs and applying them to the brain instead, such as feature visualization. (Update: we actually ended up doing this! See [Understanding and Controlling the Brain with Feature Visualization](/projects/controlling-brain-responses/).)

*For more details and results, see my CVPR 2025 paper [Visualizing and Controlling Cortical Responses Using Voxel-Weighted Activation Maximization](https://scholar.google.com/citations?view_op=view_citation&hl=en&user=da0OafkAAAAJ&sortby=pubdate&citation_for_view=da0OafkAAAAJ:ufrVoPGSRksC) or my [dissertation](https://search.proquest.com/openview/afad0c719d928c55d636640b71b28b94/1?pq-origsite=gscholar&cbl=18750&diss=y).*

*This work was also presented at the Vision Sciences Society 2022 Annual Meeting.*