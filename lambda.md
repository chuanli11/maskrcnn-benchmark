Installation
===

```bash
sudo pip3 install virtualenv

sudo apt-get install python3-dev

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



**Memory Requirement (MiB) (800 x 1333)**

| Batch Size  | Memory  |
|---|---|
| bs=1 | 6GB  |
| bs=2 | 8GB  |
| bs=4 | 24GB  |
| bs=16 | 48GB  |
| bs=32 | x  |

**Throughput (samples/sec)** 

|   | 2060  | 2070  | 2080  |  1080 Ti | 2080 Ti | TitanRTX | Quadro RTX 6000 | V100 | Quadro RTX 8000 |
|---|---|---|---|---|---|---|---|---|---|
| bs=1  | 2.85 | 3.18  | 3.88  | 3.85  | 4.96 | 5.42  |  4.95 | 5.36  | 5.17  |
| bs=2  | OOM  | 3.33  | 4.36  | 4.42  | 5.22 | 5.64  |  5.12 | 5.66  | 5.35  |
| bs=4  | OOM  | OOM  | OOM  | OOM  | OOM  | 6.14  | 5.43  | 6.35 |  5.95 |
| bs=16  | OOM  | OOM  | OOM  | OOM  | OOM  | OOM  | OOM  | OOM  | 5.84  |
| bs=32  | OOM  | OOM  | OOM  | OOM  | OOM  | OOM  | OOM  | OOM  | OOM  |



