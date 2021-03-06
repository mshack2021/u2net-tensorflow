# u2net-tensorflow

A tensorflow implementation of the [U2-Net: Going Deeper with Nested U-Structure for Salient Object Detection](https://arxiv.org/pdf/2005.09007.pdf) using Keras & Functional API

Based on the [PyTorch version](https://github.com/NathanUA/U-2-Net) by NathanUA, PDillis, vincentzhang, and chenyangh

![Network structure](examples/structure.png)

## Notebook
If you just want to play with the model, I've setup a [Google Colab Notebook](https://colab.research.google.com/drive/1bGkgDBAmn7FUX_lws3OYF8Klw80ddMN7?usp=sharing) that lets you train the model on DUTS-TR, and it's fun to watch the model learn to mask an image of the Space Needle that it's never seen before while it trains. Training takes ~60 minutes to get to noticeable results, but you should train for several hours to use it for testing. Please let me know if you have any questions.

![Network learning space needle](examples/grid.png)

## Setup 

```bash
virtualenv venv
source venv/bin/activate
pip install tensorflow matplotlib opencv-python wget
```

## Training

**OPTIONAL:** Download the [DUTS-TR](http://saliencydetection.net/duts/#org3aad434) dataset and unzip into the `data` directory to load the training set:

```bash
wget http://saliencydetection.net/duts/download/DUTS-TR.zip
unzip DUTS-TR.zip -d data
```

If [train.py](train.py) does not detect the dataset is present when run, it will automatically try to download and setup the dataset before initiating training. If you have a custom dataset, you can update `dataset_dir`, `image_dir`, and `mask_dir` in [config.py](config.py). Images will be rescaled 

To begin training, simply run:

```bash
python train.py
```

Weights are automatically saved every `save_interval` iterations to `weights/u2net.h5`. These can be overwritten by passing the appropriate arguments. See `python train.py -h` for args.

## Evaluation

Use [eval.py](eval.py)  to evaluate the model on images:

```bash
python eval.py --weights=weights/u2net.h5 --image=examples/skateboard.jpg
```

By default, the output images are saved in the `out` subdirectory

## Custom Usage

The `U2NET` class can be used to instatiate a modular instance of the U2-Net network

```python
from model.u2net import U2NET

u2net = U2NET()
out = u2net(inp)
```