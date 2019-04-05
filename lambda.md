Installation
===

```bash
sudo pip3 install virtualenv

cd maskrcnn-benchmark
virtualenv -p /usr/bin/python3.6 venv-lambda
. venv-lambda/bin/activate


pip3 install https://download.pytorch.org/whl/cu100/torch-1.0.1.post2-cp36-cp36m-linux_x86_64.whl

pip3 install torchvision

pip install ninja yacs cython matplotlib tqdm opencv-python

export INSTALL_DIR=$PWD

cd $INSTALL_DIR
git clone https://github.com/cocodataset/cocoapi.git
cd cocoapi/PythonAPI
python setup.py build_ext install

cd $INSTALL_DIR
python setup.py build develop

unset INSTALL_DIR

```


Setup Dataset
===

```bash
cd maskrcnn-benchmark
mkdir -p datasets/coco

ln -s /home/ubuntu/demo/data/coco/annotations datasets/coco/annotations 
ln -s /home/ubuntu/demo/data/coco/train2014 datasets/coco/train2014 
ln -s /home/ubuntu/demo/data/coco/val2014 datasets/coco/test2014 
ln -s /home/ubuntu/demo/data/coco/val2014 datasets/coco/val2014 

```

Train
===

```bash
CUDA_VISIBLE_DEVICES=0 python ./tools/train_net.py --config-file ./configs/e2e_mask_rcnn_R_101_FPN_1x.yaml

# use IMS_PER_BATCH to set batch size
```



**Memory Requirement (MiB)**

| Batch Size  | Memory  |
|---|---|
| bs=4 |   |
| bs=8 |   |
| bs=16 |   |
| bs=32 |   |

**Throughput (samples/sec)** 

|   | 2060  | 2070  | 2080  |  1080 Ti | 2080 Ti | TitanRTX | Quadro RTX 6000 | V100 | Quadro RTX 8000 |
|---|---|---|---|---|---|---|---|---|---|
| bs=4  |   |   | 3.88  | 3.85  |   |   |   | 4.89  | 5.17  |
| bs=8  |   |   |   |   |   |   |   |   |   |
| bs=16  |   |   |   |   |   |   |   |   | 5.84  |
| bs=32  |   |   |   |   |   |   |   |   | OOM  |



