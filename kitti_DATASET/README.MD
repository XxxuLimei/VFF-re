1. 按照mmlab的方式配置KITTI数据集  
```
mkdir ./data/kitti/ && mkdir ./data/kitti/ImageSets  

# Download data split  
wget -c  https://raw.githubusercontent.com/traveller59/second.pytorch/master/second/data/ImageSets/test.txt --no-check-certificate --content-disposition -O ./data/kitti/ImageSets/test.txt  
wget -c  https://raw.githubusercontent.com/traveller59/second.pytorch/master/second/data/ImageSets/train.txt --no-check-certificate --content-disposition -O ./data/kitti/ImageSets/train.txt  
wget -c  https://raw.githubusercontent.com/traveller59/second.pytorch/master/second/data/ImageSets/val.txt --no-check-certificate --content-disposition -O ./data/kitti/ImageSets/val.txt  
wget -c  https://raw.githubusercontent.com/traveller59/second.pytorch/master/second/data/ImageSets/trainval.txt --no-check-certificate --content-disposition -O ./data/kitti/ImageSets/trainval.txt  

python tools/create_data.py kitti --root-path ./data/kitti --out-dir ./data/kitti --extra-tag kitti --with-plane
```  


2. 运行`python -m pcdet.datasets.kitti.kitti_dataset create_kitti_infos tools/cfgs/dataset_configs/kitti_dataset.yaml`  
- 打开`tools/cfgs/dataset_configs/kitti_dataset.yaml`文件，将第二行DATA_PATH按照自己的路径进行修改；  
- 打开`pcdet.datasets.kitti.kitti_dataset.py`文件，将第477行`dataset_cfg = EasyDict(yaml.load(open(sys.argv[2])))`修改为`dataset_cfg = EasyDict(yaml.load(open(sys.argv[2]), Loader=yaml.FullLoader))`;  
- 出现错误`ImportError: cannot import name 'VoxelGenerator' from 'spconv.utils' (/home/xilm/anaconda3/lib/python3.9/site-packages/spconv/pytorch/utils.py)`,按照[这个教程](https://zhuanlan.zhihu.com/p/524235616)把spconv 2.x版本换成了v1.2.1;(注：安装Pybind11时，cmake后用`sudo make -j8`才能成功（捂脸）)  
- 
