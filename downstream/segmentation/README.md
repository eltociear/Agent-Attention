# Agent Attention for Semantic Segmentaion

Code and configuration files to reproduce semantic segmentation results of our paper. All experiments are conducted on ADE20K dataset based on [mmsegmentation](https://github.com/open-mmlab/mmsegmentation/).

## Results and Models

### UperNet

| Backbone | Pretrain | Lr Schd | mIoU | mAcc | #params | FLOPs | config | model |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Agent-Swin-T | ImageNet-1K | 160K | 46.68 | 58.53 | 61M | 954G | [config](configs/agent_swin/upernet_agent_swin_t_9-12-14-7.py) | [TsinghuaCloud](https://cloud.tsinghua.edu.cn/f/7e1321f88933403a8c49/?dl=1) |
| Agent-Swin-S | ImageNet-1K | 160K | 48.08 | 59.78 | 81M | 1043G | [config](configs/agent_swin/upernet_agent_swin_s_9-12-14-7.py) | [TsinghuaCloud]() |
| Agent-Swin-B | ImageNet-1K | 160K | 48.73 | 60.01 | 121M | 1196G | [config](configs/agent_swin/upernet_agent_swin_b_9-12-14-7.py) | [TsinghuaCloud]() |

### Semantic FPN

| Backbone | Pretrain | Lr Schd | mIoU | mAcc | #params | FLOPs | config | model |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Agent-PVT-T | ImageNet-1K | 40K | 40.18 | 51.76 | 15M | 147G | [config](configs/agent_pvt/fpn_agent_pvt_t_12-16-28-28.py) | [TsinghuaCloud](https://cloud.tsinghua.edu.cn/f/3a83b6f7c51b43c38416/?dl=1) |
| Agent-PVT-S | ImageNet-1K | 40K | 44.18 | 56.17 | 24M | 211G | [config](configs/agent_pvt/fpn_agent_pvt_s_12-16-28-28.py) | [TsinghuaCloud](https://cloud.tsinghua.edu.cn/f/63ca189330374615a19b/?dl=1) |
| Agent-PVT-M | ImageNet-1K | 40K | 44.30 | 56.42 | 40M | 321G | [config](configs/agent_pvt/fpn_agent_pvt_m_12-16-28-28.py) | [TsinghuaCloud]() |
| Agent-PVT-L | ImageNet-1K | 40K | 46.52 | 58.50 | 52M | 434G | [config](configs/agent_pvt/fpn_agent_pvt_l_12-16-28-28.py) | [TsinghuaCloud](https://cloud.tsinghua.edu.cn/f/aeddc9578f0444caa546/?dl=1) |

## Usage

### Dataset

Prepare ADE20K dataset, and change `data_root` argument in `configs/_base_/datasets/ade20k.py` to the dataset path.

### ImageNet-1K Pretrained Model

Please place ImageNet-1K pretrained models under `./data/` folder and rename them as `{MODEL_STRUCTURE}_max_acc.pth`, e.g. `agent_swin_t_max_acc.pth`.

### Installation

For convenience, we provide the conda environment file.
```
conda env create -f agent_segmentation.yaml
cd ../mmcv/
pip install -v -e .
cd ../segmentation/
pip install -v -e .
```

### Inference

```
# single-gpu testing
python tools/test.py <CONFIG_FILE> <DET_CHECKPOINT_FILE> --eval mIoU

# multi-gpu testing
tools/dist_test.sh <CONFIG_FILE> <DET_CHECKPOINT_FILE> <GPU_NUM> --eval mIoU
```

### Training

To train a detector with pre-trained models, run:
```
# single-gpu training
python tools/train.py <CONFIG_FILE>

# multi-gpu training
torchrun --nproc_per_node <GPU_NUM> tools/train.py <CONFIG_FILE> --launcher="pytorch"
```

## Citation

If you find this repo helpful, please consider citing us.

```latex
@article{han2023agent,
  title={Agent Attention: On the Integration of Softmax and Linear Attention},
  author={Han, Dongchen and Ye, Tianzhu and Han, Yizeng and Xia, Zhuofan and Song, Shiji and Huang, Gao},
  journal={arXiv preprint arXiv:2312.08874},
  year={2023}
}
```
