# CPFormer: Contrast Planes Transformer for Point Cloud Semantic Segmentation

## Installation

Clone the repository:
```bash
git clone https://github.com/Buanma/CPFormer.git
```
Create a conda virtual environment and install the corresponding dependencies according to [Point Transformer V2](https://github.com/Pointcept/PointTransformerV2/) (we test on python=3.8.15, pytorch==1.10.1, cuda==11.1, spconv-cu113==2.3.3).

Install 'sptr' for Disassembled Plane Attention:
```bash
# sptr is modified from https://github.com/dvlab-research/SphereFormer
conda activate pcr
cd libs/sptr
python setup.py install
```

Modify the spconv code to support depth-wise sparse convolution.
```bash
# PATH is the path of your pcr conda env.
# This operation replaces /PATH/pcr/lib/python3.8/site-packages/spconv/pytorch/conv.py with libs/spconv/conv.py. 
cp libs/spconv/conv.py /PATH/pcr/lib/python3.8/site-packages/spconv/pytorch/
```

## Data Preparation
Please follow the instructions in [Point Transformer V2](https://github.com/Pointcept/PointTransformerV2/) to prepare the ScanNet v2 and S3DIS datasets and link processed dataset to codebase.

The structure of the data folder should be
```
data/
  --scannet
    --train/scene*.pth
    --val/scene*.pth
    --test/scene*.pth
  --s3dis
    --Area_1/*.pth
    --Area_2
    --**
```

## Training

Run the following command to train our CPFormer.
The command will create an experiment folder according to the dataset name('-d') and experiment name('-n').
The traning config, log, checkpoints and some essential code will be save into the exp folder.
```bash
# You can adjust the gpu number according to you GPU's memory.
# For S3DIS
CUDA_VISIBLE_DEVICES=0 sh scripts/train.sh -g 1 -d s3dis -c cpformer -n cpformer
# For ScanNet20
CUDA_VISIBLE_DEVICES=0 sh scripts/train.sh -g 1 -d scannet -c cpformer -n cpformer

```

## Evaluation
Run the following command to evaluate the performance:
```bash
# With test-time-augmentation
# For S3DIS
CUDA_VISIBLE_DEVICES=0 sh ./scripts/test.sh -d s3dis -c cpformer -n test -w ./ckpts/s3dis.pth
# For ScanNet20
CUDA_VISIBLE_DEVICES=0 sh ./scripts/test.sh -d scannet -c cpformer -n test -w ./ckpts/scannet.pth
```
