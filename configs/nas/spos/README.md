# SINGLE PATH ONE-SHOT NEURAL ARCHITECTURE SEARCH WITH UNIFORM SAMPLING


## Abstract

We revisit the one-shot Neural Architecture Search (NAS) paradigm and analyze its advantages over existing NAS approaches. Existing one-shot method, however, is hard to train and not yet effective on large scale datasets like ImageNet. This work propose a Single Path One-Shot model to address the challenge in the training. Our central idea is to construct a simplified supernet, where all architectures are single paths so that weight co-adaption problem is alleviated. Training is performed by uniform path sampling. All architectures (and their weights) are trained fully and equally.
Comprehensive experiments verify that our approach is flexible and effective. It is easy to train and fast to search. It effortlessly supports complex search spaces (e.g., building blocks, channel, mixed-precision quantization) and different search constraints (e.g., FLOPs, latency). It is thus convenient to use for various needs. It achieves start-of-the-art performance on the large dataset ImageNet.

![pipeline](/docs/en/imgs/model_zoo/spos/pipeline.jpg)

## Citation

```latex
@inproceedings{guo2020single,
  title={Single path one-shot neural architecture search with uniform sampling},
  author={Guo, Zichao and Zhang, Xiangyu and Mu, Haoyuan and Heng, Wen and Liu, Zechun and Wei, Yichen and Sun, Jian},
  booktitle={European Conference on Computer Vision},
  pages={544--560},
  year={2020},
  organization={Springer}
}
```

## Results and models
|Dataset|       Supernet      | Subnet | Params(M) | Flops(G) | Top-1 (%) | Top-5 (%) | Config | Download | Remarks |
|:---------------------:|:---------------------:|:------:|:---------:|:--------:|:---------:|:---------:|:------:|:---------|:---------:|
|ImageNet|   ShuffleNetV2      |[mutable](https://openmmlab-share.oss-cn-hangzhou.aliyuncs.com/mmrazor/v0.1/nas/spos/spos_shufflenetv2_subnet_8xb128_in1k/spos_shufflenetv2_subnet_8xb128_in1k_flops_0.33M_acc_73.87_20211222-454627be_mutable_cfg.yaml?versionId=CAEQHxiBgICw5b6I7xciIGY5MjVmNWFhY2U5MjQzN2M4NDViYzI2YWRmYWE1YzQx)|    3.35    |  0.33        |     73.87  |    91.6   |[config](./spos_shufflenetv2_subnet_8xb128_in1k.py)|[model](https://openmmlab-share.oss-cn-hangzhou.aliyuncs.com/mmrazor/v0.1/nas/spos/spos_shufflenetv2_subnet_8xb128_in1k/spos_shufflenetv2_subnet_8xb128_in1k_flops_0.33M_acc_73.87_20211222-1f0a0b4d.pth?versionId=CAEQHxiBgIDK5b6I7xciIDM1YjIwZjQxN2UyMDRjYjA5YTM5NTBlMGNhMTdkNjI2) &#124; [log](https://openmmlab-share.oss-cn-hangzhou.aliyuncs.com/mmrazor/v0.1/nas/spos/spos_shufflenetv2_subnet_8xb128_in1k/spos_shufflenetv2_subnet_8xb128_in1k_flops_0.33M_acc_73.87_20211222-1f0a0b4d.log.json?versionId=CAEQHxiBgIDr9cuL7xciIDBmOTZiZGUyYjRiMDQ5NzhhZjY0NWUxYmUzNDlmNTg5)| MMRazor searched

**Note**:
1. There are some small differences in our experiment in order to be consistent with other repos in OpenMMLab. For example,
normalize images in data preprocessing; resize by cv2 rather than PIL in training; dropout is not used in network.
2. We also retrain the subnet reported in paper with their official code, Top-1 is 73.6 and Top-5 is 91.6
## Getting Started
### Supernet pre-training on ImageNet
```bash
python ./tools/mmcls/train_mmcls.py \
  configs/nas/spos/spos_shufflenet_supernet_imagenet.py \
  --work-dir $WORK_DIR
```
### Search for subnet on the trained supernet
```bash
python ./tools/mmcls/search_mmcls.py \
  configs/nas/spos/spos_shufflenet_evolution_search_imagenet.py \
  $STEP1_CKPT \
  --work-dir $WORK_DIR
```

### Subnet retraining on ImageNet
```bash
python ./tools/mmcls/train_mmcls.py \
  configs/nas/spos/spos_shufflenet_subnet_imagenet.py \
  --work-dir $WORK_DIR \
  --cfg-options algorithm.mutable_cfg=$STEP2_SUBNET_YAML
```
