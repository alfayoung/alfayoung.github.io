---
title: "Interleave-VLA"
collection: publications
category: conferences
permalink: /publication/2025-05-15-interleave-vla
excerpt: 'This paper introduces Interleave-VLA, a novel robot learning paradigm that leverages interleaved image-text instructions to enhance robot manipulation capabilities in unseen scenarios.'
date: 2025-05-15
venue: 'ICRA (International Conference on Robotics and Automation) 2025 Safe-VLM Workshop Spotlight'
paperurl: 'https://arxiv.org/abs/2505.02152'
citation: '@misc{fan2025interleavevlaenhancingrobotmanipulation,
      title={Interleave-VLA: Enhancing Robot Manipulation with Interleaved Image-Text Instructions}, 
      author={Cunxin Fan and Xiaosong Jia and Yihang Sun and Yixiao Wang and Jianglan Wei and Ziyang Gong and Xiangyu Zhao and Masayoshi Tomizuka and Xue Yang and Junchi Yan and Mingyu Ding},
      year={2025},
      eprint={2505.02152},
      archivePrefix={arXiv},
      primaryClass={cs.RO},
      url={https://arxiv.org/abs/2505.02152}, 
}'
---

The rise of foundation models paves the way for generalist robot policies in the physical world. Existing methods relying on text-only instructions often struggle to generalize to unseen scenarios. We argue that interleaved image-text inputs offer richer and less biased context and enable robots to better handle unseen tasks with more versatile human-robot interaction. Building on this insight, Interleave-VLA, the first robot learning paradigm capable of comprehending interleaved image-text instructions and directly generating continuous action sequences in the physical world, is introduced. It offers a natural, flexible, and model-agnostic paradigm that extends state-of-the-art vision-language-action (VLA) models with minimal modifications while achieving strong zero-shot generalization. Interleave-VLA also includes an automatic pipeline that converts text instructions from Open X-Embodiment into interleaved image-text instructions, resulting in a large-scale real-world interleaved embodied dataset with 210k episodes. Comprehensive evaluation in simulation and the real world shows that Interleave-VLA offers two major benefits: (1) improves out-of-domain generalization to unseen objects by 2× compared to text input baselines, (2) supports flexible task interfaces and diverse instructions in a zero-shot manner, such as hand-drawn sketches. We attribute Interleave-VLA's strong zero-shot capability to the use of instruction images, which effectively mitigate hallucinations, and the inclusion of heterogeneous multimodal datasets, enriched with Internet-sourced images, offering potential for scalability. [Our project site](https://interleave-vla.github.io/Interleave-VLA-Anonymous/) has more information.