# CISiP-CoCa - Compact Image Captioning

![Tests](https://github.com/CISiPLab/cisip-CoCa/actions/workflows/tests.yml/badge.svg)
![Black](https://github.com/CISiPLab/cisip-CoCa/actions/workflows/black.yml/badge.svg)
[![Documentation Status](https://readthedocs.org/projects/cisip-coca/badge/?version=latest)](https://cisip-coca.readthedocs.io/en/latest/?badge=latest)


## Introduction
Compact Image Captioning (CoCa) is an open source image captioning project release by Center of Image and Signal Processing Lab (CISiP Lab), Universiti Malaya. This project is to promote Green Computer Vision to reduce carbon footprint, as well as to make computer vision research (in this case - image captioning research) accessible to universities, research labs and individual practitioners with limited financial resources.

## Get Started

[Please refer to the documentation](https://cisip-coca.readthedocs.io/en/latest/).


## Features

* Captioning models built using PyTorch
    * [Up-Down LSTM](http://openaccess.thecvf.com/content_cvpr_2018/html/Anderson_Bottom-Up_and_Top-Down_CVPR_2018_paper.html)
    * [Object Relation Transformer](https://papers.nips.cc/paper/9293-image-captioning-transforming-objects-into-words.pdf)
    * A Compact Object Relation Transformer (ACORT)
* Unstructured weight pruning
    * [Supermask Pruning (SMP, end-to-end pruning)](https://arxiv.org/abs/2110.03298)
    * [Gradual magnitude pruning](https://arxiv.org/abs/1710.01878)
    * [Lottery ticket](https://arxiv.org/abs/1803.03635)
    * One-shot magnitude pruning ([paper 1](https://arxiv.org/abs/1506.02626), [paper 2](https://arxiv.org/abs/1606.09274))
    * [Single-shot Network Pruning (SNIP)](https://arxiv.org/abs/1810.02340)
* Self-Critical Sequence Training (SCST)]()
    * Random sampling + Greedy search baseline: [vanilla SCST](https://openaccess.thecvf.com/content_cvpr_2017/html/Rennie_Self-Critical_Sequence_Training_CVPR_2017_paper.html)
    * Beam search sampling + Greedy search baseline: à la [Up-Down](http://openaccess.thecvf.com/content_cvpr_2018/html/Anderson_Bottom-Up_and_Top-Down_CVPR_2018_paper.html)
    * Random sampling + Sample mean baseline: [arxiv paper](https://arxiv.org/abs/2003.09971)
    * Beam search sampling + Sample mean baseline: à la [M2 Transformer](http://openaccess.thecvf.com/content_CVPR_2020/html/Cornia_Meshed-Memory_Transformer_for_Image_Captioning_CVPR_2020_paper.html)
    * Optimise CIDEr and/or BLEU scores with custom weightage
    * Based on [ruotianluo/self-critical.pytorch](https://github.com/ruotianluo/self-critical.pytorch/tree/3.2)
* Multiple captions per image during teacher-forcing training
    * Reduce training time: run encoder once, optimize on multiple training captions
* Incremental decoding (Transformer with attention cache)
* Data caching during training
    * Training examples will be cached in memory to reduce disk I/O
    * With sufficient memory, the entire training set can be loaded from memory after the first epoch
    * Memory usage can be controlled via `cache_min_free_ram` flag
* `coco_caption` in Python 3
    * Based on [salaniz/pycocoevalcap](https://github.com/salaniz/pycocoevalcap/tree/ad63453cfab57a81a02b2949b17a91fab1c3df77)
* Tokenizer based on `sentencepiece`
    * Word
    * [Radix encoding](https://github.com/jiahuei/COMIC-Compact-Image-Captioning-with-Attention)
    * _(untested)_ Unigram, BPE, Character
* Datasets
    * MS-COCO
    * _(contributions welcome)_ Flickr8k, Flickr30k, InstaPIC-1.1M


## Pre-trained Sparse and ACORT Models

The checkpoints are [available at this repo](https://github.com/jiahuei/sparse-captioning-checkpoints).

Soft-attention models implemented in TensorFlow 1.9 are available at [this repo](https://github.com/jiahuei/tf-sparse-captioning).


## CIDEr score of pruning methods (on MS-COCO dataset)

### Up-Down (UD)

| Sparsity | NNZ | Dense Baseline | SMP | Lottery ticket (class-blind) | Lottery ticket (class-uniform) | Lottery ticket (gradual) | Gradual pruning | Hard pruning (class-blind) | Hard pruning (class-distribution) | Hard pruning (class-uniform) | SNIP |
|---|---|---|---|---|---|---|---|---|---|---|---|
| 0.950 | 2.7 M | 111.3 | **112.5** | - | 107.7 | 109.5 | 109.7 | - | 110.0 | 110.2 | 38.2 |
| 0.975 | 1.3 M | 111.3 | **110.6** | - | 103.8 | 106.6 | 107.0 | - | 105.9 | 105.4 | 34.7 |
| 0.988 | 0.7 M | 111.3 | **109.0** | - | 99.3 | 102.2 | 103.4 | - | 101.3 | 100.5 | 32.6 |
| 0.991 | 0.5 M | 111.3 | **107.8** |  |  |  |  |  |  |  |  |

### Object Relation Transformer (ORT)

| Sparsity | NNZ | Dense Baseline | SMP | Lottery ticket (gradual) | Gradual pruning | Hard pruning (class-blind) | Hard pruning (class-distribution) | Hard pruning (class-uniform) | SNIP |
|---|---|---|---|---|---|---|---|---|---|
| 0.950 | 2.8 M | 114.7 | 113.7 | **115.7** | 115.3 | 4.1 | 112.5 | 113.0 | 47.2 |
| 0.975 | 1.4 M | 114.7 | **113.7** | 112.9 | 113.2 | 0.7 | 106.6 | 106.9 | 44.0 |
| 0.988 | 0.7 M | 114.7 | **110.7** | 109.8 | 110.0 | 0.9 | 96.9 | 59.8 | 37.3 |
| 0.991 | 0.5 M | 114.7 | **109.3** | 107.1 | 107.0 |  |  |  |  |


## Acknowledgements

* SCST, Up-Down: [ruotianluo/self-critical.pytorch](https://github.com/ruotianluo/self-critical.pytorch/tree/3.2)
* Object Relation Transformer: [yahoo/object_relation_transformer](https://github.com/yahoo/object_relation_transformer)
* `coco_caption` in Python 3: [salaniz/pycocoevalcap](https://github.com/salaniz/pycocoevalcap/tree/ad63453cfab57a81a02b2949b17a91fab1c3df77)


## Citation

If you find this work useful for your research, please cite
```
@article{tan2021end,
  title={End-to-End Supermask Pruning: Learning to Prune Image Captioning Models},
  author={Tan, Jia Huei and Chan, Chee Seng and Chuah, Joon Huang},
  journal={Pattern Recognition},
  pages={108366},
  year={2021},
  publisher={Elsevier},
  doi={10.1016/j.patcog.2021.108366}
}
```

## Contribution
We welcome the contributions to improve this project, in particular on other datasets - such as Flickr8k, Flickr30k, InstaPIC-1.1M etc. Please file your suggestions/issues by creating new issues or send us a pull request for your new changes/improvement/features/fixes.

## License and Copyright
The project is open source under BSD-3 license (see the ``` LICENSE ``` file).

&#169;2021 Universiti Malaya.

