# Image-Based_Patent_Retrieval
Learning Efficient Representations for Image-Based Patent Retrieval (https://arxiv.org/abs/2308.13749)

# Approach_Description

## Motivation

Patent retrieval aims to retrieve the same patent image despite different views. We think the setting is similar to person re-identification (re-ID). We compare these two tasks in the following. 

* Person re-ID. Different views, clothing color important
* Patent retrieval. Different views, black white image, texture important

We use basic knowledge from person re-ID, e.g., large backbones and loss functions. We also test with different augmentations and find an interesting conclusion that separates patent retrieval from person re-ID. To further improve performance, we use K-reciprocal (KR) rerank to retrieve hard samples with neighborhood features.

## Related Works

# Patent retrieval

Patent retrieval has many potential uses, e.g., intellectual property protection. DeepPatent is a very large scale dataset collected by [1]. The paper also develops a baseline deep learning model, named PatentNet, based on best practices for training retrieval models for static image. The dataset and methods provide valuable information for our competition.

# Person re-ID

Person re-ID has developed rapidly these years. The backbones usually share with image classification backbones. The loss functions include softmax, triplet and arcface loss, etc. Also, post-processing techniques such as KR rerank have also been used since people of different views are sometimes too difficult for a model. Using extra neighbors would ease the problem.

## Method

We use EfficientNet as CNN backbones (EffNetB5-B7) and SwinBase transformer as a transformer baseline. The image resolution is 768x768. We use arcface loss with scale=20 and margin=0.5. For KR rerank, we use k1=10 and k2=3.

## Experiments

# Setting
Since the DeepPatent data is split into train/val/test, we have two options.

* Development setting: Only train subset for training and val subset for evaluation
* Final setting: train+val subsets for training and codalab_test subset for submission.

In this guidance, we only report results from development setting. Also, we report mAP@all rather than mAP@10 as the competition.

# General findings

We use EfficientNetB0 + 256x256 input size + softmax loss as a baseline. We replace the backbone, resolution and loss and show the performance improvement. We find that image resolution and loss function play an important role.

| Setting        | mAP    | Rank-5 |
| :-----         | :----- | :----- |
| B0-256-softmax | 0.5540 | 0.9140 |
| B0-256-arcface | 0.7120 | 0.9580 |
| B0-512-arcface | 0.8770 | 0.9900 |
| B5-512-arcface | 0.8990 | 0.9930 |

# Interesting findings

We list more interesting findings here. We think these findings make patent retrieval different from classic image retrieval or person re-ID.

| Setting        | mAP    | Rank-5 |
| :-----         | :----- | :----- |
| B0-256-softmax | 0.5540 | 0.9140 |
| gray scale     | 0.5350 | 0.9050 |
| scale crop     | 0.4000 | 0.8250 |
| rotation       | 0.4600 | 0.8590 |

Although patent images are provided in gray-scale, we find that training on gray-scale images hurts the performance. The reason is that we lack suitable pretrained models in gray-scale. Also the common data augmentation strategy, scale+crop hurt the performance greatly. We observe the patent data has the same line width regardless of the content. Data augmentation on the scales might break such line width regularization.  

## Claim

No external data is used except for Imagenet data.

## Reference

[1] DeepPatent: Large scale patent drawing recognition and retrieval


```
@inproceedings{wang2023learning,
  title={Learning efficient representations for image-based patent retrieval},
  author={Wang, Hongsong and Zhang, Yuqi},
  booktitle={Chinese Conference on Pattern Recognition and Computer Vision (PRCV)},
  pages={15--26},
  year={2023},
  organization={Springer}
}
