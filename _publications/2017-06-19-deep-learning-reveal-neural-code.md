---
title: "Using deep learning to reveal the neural code for images in primary visual cortex"
collection: publications
permalink: /publication/using-deep-learning-to-reveal
excerpt: 'We train deep convolutional neural networks to predict the firing rates of V1 neurons in response to natural images'
date: 2017-06-19
venue: 'Journal of Vision'
arxiv: "https://arxiv.org/pdf/1706.06208"
doi: 10.1167/19.4.29
code: https://github.com/jzlab/v1_predictor
header:
  teaser:  'jov/network_diagram.png'
citation: 'Kindel, W; <strong>Christensen, E</strong>; Zylberberg, J; (2019).'
---

Primary visual cortex (V1) is the first stage of cortical image processing, and a major effort in systems neuroscience is devoted to understanding how it encodes information about visual stimuli. Within V1, many neurons respond selectively to edges of a given preferred orientation: these are known as simple or complex cells, and they are well-studied. Other neurons respond to localized center-surround image features. Still others respond selectively to certain image stimuli, but the specific features that excite them are unknown. Moreover, even for the simple and complex cells-- the best-understood V1 neurons-- it is challenging to predict how they will respond to natural image stimuli. Thus, there are important gaps in our understanding of how V1 encodes images. To fill this gap, we train deep convolutional neural networks to predict the firing rates of V1 neurons in response to natural image stimuli, and find that 15% of these neurons are within 10% of their theoretical limit of predictability. For these well predicted neurons, we invert the predictor network to identify the image features (receptive fields) that cause the V1 neurons to spike. In addition to those with previously-characterized receptive fields (Gabor wavelet and center-surround), we identify neurons that respond predictably to higher-level textural image features that are not localized to any particular region of the image.

<a href='http://www.jzlab.org/Christensen_JSleepResearch2018_LFP_ANN_DBS.pdf'>[PDF]</a>
