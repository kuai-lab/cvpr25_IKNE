# Insightful Instance Features for 3D Instance Segmentation (CVPR 2025)

Official PyTorch implementation of the CVPR 2025 paper **Insightful Instance Features for 3D Instance Segmentation**

Wonseok Roh, Hwanhee Jung, Giljoo Nam, Dong In Lee, Hyeongcheol Park, Sang Ho Yoon, Jungseock Joo, Sangpil Kim

#### Kernel-based IKNE Architecture
<div align="center">
  <img src="figs/IKNE_kernel.png"/>
</div>

#### Transformer-based IKNE Architecture
<div align="center">
  <img src="figs/IKNE_trans.png"/>
</div>

</br>

# Get Started

## 🛠 Environment

Install dependencies
```
# install attention_rpe_ops
cd lib/attention_rpe_ops && python3 setup.py install && cd ../../

# install pointgroup_ops
cd ikne/lib && python3 setup.py develop && cd ../../

# install IKNE
python3 setup.py develop

# install other dependencies
pip install -r requirements.txt
```

Note: Make sure you have installed `gcc` and `cuda`, and `nvcc` can work (if you install cuda by conda, it won't provide nvcc and you should install cuda manually.)

👾 Our code is currently based on a Transformer architecture. 👾

## 📦 Datasets Preparation

### ScanNetv2
(1) Download the [ScanNetv2](http://www.scan-net.org/) dataset.

(2) Put the data in the corresponding folders. 
* Copy the files `[scene_id]_vh_clean_2.ply`,  `[scene_id]_vh_clean_2.labels.ply`,  `[scene_id]_vh_clean_2.0.010000.segs.json`  and `[scene_id].aggregation.json`  into the `dataset/scannetv2/train` and `dataset/scannetv2/val` folders according to the ScanNet v2 train/val [split](https://github.com/ScanNet/ScanNet/tree/master/Tasks/Benchmark). 

* Copy the files `[scene_id]_vh_clean_2.ply` into the `dataset/scannetv2/test` folder according to the ScanNet v2 test [split](https://github.com/ScanNet/ScanNet/tree/master/Tasks/Benchmark). 

* Put the file `scannetv2-labels.combined.tsv` in the `dataset/scannetv2` folder.

The dataset files are organized as follows.
```
PointGroup
├── dataset
│   ├── scannetv2
│   │   ├── train
│   │   │   ├── [scene_id]_vh_clean_2.ply & [scene_id]_vh_clean_2.labels.ply & [scene_id]_vh_clean_2.0.010000.segs.json & [scene_id].aggregation.json
│   │   ├── val
│   │   │   ├── [scene_id]_vh_clean_2.ply & [scene_id]_vh_clean_2.labels.ply & [scene_id]_vh_clean_2.0.010000.segs.json & [scene_id].aggregation.json
│   │   ├── test
│   │   │   ├── [scene_id]_vh_clean_2.ply 
│   │   ├── scannetv2-labels.combined.tsv
```

(3) Generate input files `[scene_id]_inst_nostuff.pth` for instance segmentation.
```
cd dataset/scannetv2
python prepare_data_inst_with_normal.py.py --data_split train
python prepare_data_inst_with_normal.py.py --data_split val
python prepare_data_inst_with_normal.py.py --data_split test
```

## 🎾 Training

### ScanNetv2
```
python3 tools/train.py configs/scannet/ikne_scannet.yaml
```

## ⛳ Validation
```
python3 tools/train.py configs/scannet/ikne_scannet.yaml --resume [MODEL_PATH] --eval_only
```

## 🏆 Performance

#### mAP Scores on ScanNetV2 validation Dataset
| Method           | mAP | mAP50 |
|------------------|---------|---------|
| Mask3D           | 55.2    | 73.7    |
| QueryFormer      | 56.5    | 74.2    |
| MAFT             | 59.9    | 76.5    |
| EASE             | 60.2    | 77.2    |
| **IKNE (Ours)**  | **62.9**    | **81.8**    |

