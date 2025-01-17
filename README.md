## P-CNN : Prototype-CNN for Few-Shot Object Detection in Remote Sensing Images
Code for reproducing the results in the following paper, and the code is built on top of [MetaR-CNN](https://github.com/yanxp/MetaR-CNN)

P-CNN : Prototype-CNN for Few-Shot Object Detection in Remote Sensing Images

Gong Cheng, Bowei Yan, Peizhen Shi, Ke Li, Xiwen Yao, Lei Guo, and Junwei Han

<p align=center><img src="Prototype-CNN.jpg"/></p>

### License

For Academic Research Use Only!

### Requirements

+ python packages
  
  + Python = 3.6
  
  + PyTorch = 0.3.1
    
    *This project can not support pytorch 0.4, higher version will not recur results.*

  + Torchvision >= 0.2.0

  + cython

  + pyyaml

  + easydict

  + opencv-python

  + matplotlib

  + numpy

  + scipy

  + tensorboardX
  
+ CUDA 8.0

+ gcc >= 4.9

### Getting Started

Clone the repo:


    https://https://github.com/Ybowei/P-CNN.git



### Compilation

Compile the CUDA dependencies:


    cd {repo_root}/lib
    sh make.sh


It will compile all the modules you need, including NMS, ROI_Pooing, ROI_Crop and ROI_Align



### Data Preparation

create a data folder under the repo,

    cd {repo_root}
    mkdir data
    
**DIOR**：Please download the [DIOR dataset](https://gcheng-nwpu.github.io/).
After downloading the data, create softlinks in the folder data/.

please put the four base classes [splits](https://github.com/BoweiY/P-CNN/tree/main/split) into DIOR ImageSets/Main dirs.

### Training
We used [ResNet101](https://www.dropbox.com/s/iev3tkbz5wyyuz9/resnet101_caffe.pth?dl=0) pretrained model on ImageNet in our experiments. Download it and put it into the data/pretrained_model/.

for example, if you want to train the first split of base and novel class with meta learning, just run:

#### the first phase
```sh
$>CUDA_VISIBLE_DEVICES=0 python train_pcnn.py --dataset dior --epochs 21 --bs 4 --nw 8 --log_dir checkpoint --save_dir models/meta/first --meta_type 1 --meta_train True --meta_loss True 
```
#### the second phase
```sh
$>CUDA_VISIBLE_DEVICES=0 python train_pcnn.py --dataset dior --epochs 30 --bs 4 --nw 8 --log_dir checkpoint --save_dir models/meta/first --r True --checksession 3 --checkepoch 20 --checkpoint 1898 --phase 2 --shots 10 --meta_train True --meta_loss True --meta_type 1
```
### Testing

if you want to evaluate the performance of meta trained model, simply run:
```sh
$>CUDA_VISIBLE_DEVICES=0 python test_pcnn.py --dataset dior --net Prototypecnn --load_dir models/meta/first  --checksession 10 --checkepoch 30 --checkpoint 78 --shots 10  --meta_type 1 --meta_test True --meta_loss True --phase 2
```

### Citation

```
@ARTICLE{9435769,
  author={Cheng, Gong and Yan, Bowei and Shi, Peizhen and Li, Ke and Yao, Xiwen and Guo, Lei and Han, Junwei},
  journal={IEEE Transactions on Geoscience and Remote Sensing}, 
  title={Prototype-CNN for Few-Shot Object Detection in Remote Sensing Images}, 
  year={2021},
  volume={},
  number={},
  pages={1-10},
  doi={10.1109/TGRS.2021.3078507}}
```

### Contact

If you have any questions about this repo, please feel free to contact [ybw123w@163.com].
    
