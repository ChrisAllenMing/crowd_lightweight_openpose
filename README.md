# Training lightweight openpose on CrowdPose

This repository trains lightweight OpenPose ([Real-time 2D Multi-Person Pose Estimation on CPU: Lightweight OpenPose](https://arxiv.org/pdf/1811.12004.pdf)) on CrowdPose dataset ([CrowdPose: Efficient Crowded Scenes Pose Estimation and A New Benchmark](https://arxiv.org/abs/1812.00324)), and it mainly follows the work of [Daniil Osokin](https://github.com/Daniil-Osokin/lightweight-human-pose-estimation.pytorch). 

## Table of Contents

* [Requirements](#requirements)
* [Prerequisites](#prerequisites)
* [Training](#training)

## Requirements

* Ubuntu 16.04
* Python 3.6
* PyTorch 0.4.1 (should also work with 1.0, but not tested)

## Prerequisites

1. Download CrowdPose dataset: [train/validation/test images + annotations](https://github.com/Jeff-sjtu/CrowdPose) to <CP_HOME>.
2. Install the CrowdPose API: run `sh install.sh` under CrowdPose/crowdpose-api/PythonAPI/

## Training

1. Convert train annotations in internal format. Run `python scripts/prepare_cp_train_labels.py --labels <CP_HOME>/json/crowdpose_train.json`. It will produce `prepared_cp_train_annotation.pkl` with converted in internal format annotations.

2. To train from MobileNet weights, run `python train_cp.py --train-images-folder <CP_HOME>/images/ --prepared-train-labels prepared_cp_train_annotation.pkl --checkpoint-path <path_to>/mobilenet_sgd_68.848.pth.tar --from-mobilenet`

3. Next, to train from checkpoint from previous step, run `python train_cp.py --train-images-folder <CP_HOME>/images/ --prepared-train-labels prepared_cp_train_annotation.pkl --checkpoint-path <path_to>/checkpoint_iter_420000.pth.tar --weights-only`

4. Finally, to train from checkpoint from previous step and 3 refinement stages in network, run `python train_cp.py --train-images-folder <CP_HOME>/images/ --prepared-train-labels prepared_cp_train_annotation.pkl --checkpoint-path <path_to>/checkpoint_iter_280000.pth.tar --weights-only --num-refinement-stages 3`. We took checkpoint after 370000 iterations as the final one.
