# SOD-RepCIN

Implementation of paper - [Article Title]{SOD-RepCIN: A Re-parameterized Multi-scale Compact Feature Extraction Model for Small Object Detection](-)

[![arxiv.org](-)](-)
[![Hugging Face Spaces](-)](-)
[![Hugging Face Spaces](-)](-)


## Performance 

MS VisDrone

| Model | Test Size | AP<sup>val</sup> | AP<sub>50</sub><sup>val</sup> | AP<sub>50-95</sub><sup>val</sup> | Param. | FLOPs |
| :-- | :-: | :-: | :-: | :-: | :-: | :-: |
| [**SOD-RepCIN-T**]() | 640 | **46.3%** | **33.3%** | **34.5%** | **4.2M** | **21.5G** |
| [**SOD-RepCIN-S**]() | 640 | **52.6%** | **39.5%** | **41.3%** | **16.0M** | **80.1G** |
| [**SOD-RepCIN-M**]() | 640 | **57.9%** | **43.5%** | **46.1%** | **40.30M** | **179.2G** |
| [**SOD-RepCIN-C**]() | 640 | **59.2%** | **45.7%** | **48.1%** | **65.7M** | **355.1G** |


## Installation
conda create -n SOD-RepCIN python=3.11 <br>

conda activate SOD-RepCIN <br>

pip install -r requirements.txt

</details>


## Evaluation

[`SOD-RepCIN-T.pt`](https://github.com/magic524/SOD-RepCIN/releases/download/download/SOD-RepCINt_vd200.pt) [`SOD-RepCIN-S.pt`](https://github.com/magic524/SOD-RepCIN/releases/download/download/SOD-RepCINs_vd200.pt) [`SOD-RepCIN-M.pt`](https://github.com/magic524/SOD-RepCIN/releases/download/download/SOD-RepCINm_vd200.pt.pt) [`SOD-RepCIN-C.pt`](https://github.com/magic524/SOD-RepCIN/releases/download/download/SOD-RepCINc_vd200.pt) 
``` shell
# evaluate converted yolov9 models
python val_dual.py --data data/VisDrone.yaml --img 640 --batch 32 --conf 0.001 --iou 0.7 --device 0 --weights '.SOD-RepCIN-C.pt' --save-json --name SOD-RepCIN-C_640_val

# evaluate yolov9 models
# python val_dual.py --data data/VisDrone.yaml --img 640 --batch 4 --conf 0.001 --iou 0.7 --device 0 --weights '.SOD-RepCIN-C.pt' --save-json --name SOD-RepCIN-C_640_val


## Training

Data preparation
https://docs.ultralytics.com/datasets/detect/visdrone/#what-are-the-main-subsets-of-the-visdrone-dataset-and-their-applications


Single GPU training

``` shell
# train SDO-RepCIN models
python train_dual.py --workers 8 --device 0 --batch 4 --data data/VisDrone.yaml --img 640 --cfg models/detect/SOD-RepCINt.yaml --weights '' --name SOD-RepCINt --hyp hyp.scratch-high.yaml --min-items 0 --epochs 200 --close-mosaic 15

# train gelan models
# python train.py --workers 8 --device 0 --batch 32 --data data/coco.yaml --img 640 --cfg models/detect/gelan-c.yaml --weights '' --name gelan-c --hyp hyp.scratch-high.yaml --min-items 0 --epochs 500 --close-mosaic 15
```

Multiple GPU training

``` shell
# train SOD-RepCINc models
python -m torch.distributed.launch --nproc_per_node 8 --master_port 9527 train_dual.py --workers 8 --device 0,1,2,3,4,5,6,7 --sync-bn --batch 128 --data data/VisDrone.yaml --img 640 --cfg models/detect/SOD-RepCINc.yaml --weights '' --name SOD-RepCINc --hyp hyp.scratch-high.yaml --min-items 0 --epochs 200 --close-mosaic 15


```





## Inference


``` shell

# inference yolov9 models
python detect_dual.py --source '.data/images/9999970_00000_d_0000012.jpg' --img 640 --device 0 --weights './SOD-RepCIN-C.pt' --name SOD-RepCIN-C.pt_640_detect

# inference gelan models
# python detect.py --source './data/images/horses.jpg' --img 640 --device 0 --weights './gelan-c.pt' --name gelan_c_c_640_detect
```


## Citation

```
-
```

```
@article{wang2024yolov9,
  title={{YOLOv9}: Learning What You Want to Learn Using Programmable Gradient Information},
  author={Wang, Chien-Yao  and Liao, Hong-Yuan Mark},
  booktitle={arXiv preprint arXiv:2402.13616},
  year={2024}
}
```




## Acknowledgements

<details><summary> <b>Expand</b> </summary>

* [https://github.com/WongKinYiu/yolov9](https://github.com/WongKinYiu/yolov9)
* [https://github.com/THU-MIG/yolov10](https://github.com/THU-MIG/yolov10)
* [https://github.com/ChipsGuardian/DualConv](https://github.com/ChipsGuardian/DualConv)
* [https://github.com/megvii-model/RepVGG](https://github.com/megvii-model/RepVGG)
* [https://github.com/DingXiaoH/RepVGG](https://github.com/DingXiaoH/RepVGG)
* [https://github.com/ultralytics/ultralytics](https://github.com/ultralytics/ultralytics)

</details>
