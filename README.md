# Palette: Image-to-Image Diffusion Models

[Paper](https://arxiv.org/pdf/2111.05826.pdf ) |  [Project](https://iterative-refinement.github.io/palette/ )

## Brief

This is an adaptation of an unofficial implementation of **Palette: Image-to-Image Diffusion Models** by **Pytorch** (https://github.com/Janspiry/Palette-Image-to-Image-Diffusion-Models). To use the code in Google Colab, one can simply do 

```
%cd /content/
!git clone https://github.com/zacqoo/Palette-Experiment.git
```

There are some implementation details with paper descriptions:

- We adapted the U-Net architecture used in  `Guided-Diffusion`, which give a substantial boost to sample quality.
- We used the attention mechanism in low-resolution features (16×16) like vanilla `DDPM`.
- We encode the $\gamma$ rather than $t$ in `Palette` and embed it with affine transformation.
- We fix the variance $Σ_\theta(x_t, t)$ to a constant during the inference as described in `Palette`.

## Status

### Code
- [x] Diffusion Model Pipeline
- [x] Train/Test Process
- [x] Save/Load Training State
- [x] Logger/Tensorboard
- [x] Multiple GPU Training (DDP)
- [x] EMA
- [x] Metrics (now for FID, IS)
- [x] Dataset (different masks proposed)

### Visuals

## Usage
### Environment
```python
pip install -r requirements.txt
```

### Data Prepare

The images are in 

```
Palette-Satellite/datasets/train
```

Or you can modift the configure file json to point to your data. Take the following as an example:

```yaml
"which_dataset": {  // import designated dataset using arguments 
    "name": ["data.dataset", "InpaintDataset"], // import Dataset() class
    "args":{ // arguments to initialize dataset
    	"data_root": "your data path",
    	"data_len": -1,
    	"mask_mode": "hybrid"
    } 
},
``` 
More choices about **dataloader** and **validation split** also can be found in `datasets`  part of configure file.

Note that we can create different mask_modes.

### Model and networks options 

All customerized modifications could be done by modifying the following file :

```
inpainting_m.json
```

For example : the input_channel and out_put channel of Unet

### Training/Resume Training

```

1. Run the script:

```python
python run.py -p train -c config/inpainting_m.json
```

We test the U-Net backbone used in `SR3` and `Guided Diffusion`,  and `Guided Diffusion` one have a more robust performance in our current experiments.  More choices about **backbone**, **loss** and **metric** can be found in `which_networks`  part of configure file.

### Test
1. Set the model path following the steps in **Resume Training** part.
2. Run the script:
```python
python run.py -p test -c config/inpainting_m.json
```

### Evaluation
1. Create two folders saving ground truth images and sample images, and their file names need to correspond to each other.

2. Run the script:

```python
python eval.py -s [ground image path] -d [sample image path]
```



## Acknowledge
Our work is based on the following theoretical works:
- [Denoising Diffusion Probabilistic Models](https://arxiv.org/pdf/2006.11239.pdf)
- [Palette: Image-to-Image Diffusion Models](https://arxiv.org/pdf/2111.05826.pdf)
- [Diffusion Models Beat GANs on Image Synthesis](https://arxiv.org/abs/2105.05233)

and we are benefiting a lot from the following projects:
- [openai/guided-diffusion](https://github.com/openai/guided-diffusion)
- [LouisRouss/Diffusion-Based-Model-for-Colorization](https://github.com/LouisRouss/Diffusion-Based-Model-for-Colorization)
