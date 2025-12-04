# scSplit: Bringing Severity Cognizance to Image Decomposition in Fluorescence Microscopy

## Overview

This is the official implementation of **scSplit: Bringing Severity Cognizance to Image Decomposition in Fluorescence Microscopy**, published at NeurIPS 2025. [Paper link](https://arxiv.org/abs/2503.22983).

## Installation
1. Install [mamba](https://github.com/mamba-org/mamba).
2. Execute installation.sh.

After installation, one needs to do `mamba activate split_hpc` to activate the environment and one can subsequently start to train or evaluate the models. 

## Training
### For $Gen_i$ networks
```
python /home/ashesh.ashesh/code/DiffSplitting/split.py -c /home/ashesh.ashesh/code/DiffSplitting/config/hagen_indiSplit.json -enable_wandb
```

### For $Reg$ network
```
python /home/ashesh.ashesh/code/DiffSplitting/time_prediction_training.py  --config=/home/ashesh.ashesh/code/DiffSplitting/config/ht_t24_time_predictor.json -enable_wandb
```

## Evaluation
Evaluation is done by running the notebooks. For some of the tasks, it can take more time and so we execute the notebooks as scripts.

### Synthetically Summed Inputs

In this case the argument `--mixing_t_ood=0.5` fixes the desired mixing-ratio in the inputs.
```
python notebooks/evaluate_notebook.py  --mmse_count=10  --notebook=/home/ashesh.ashesh/code/DiffSplitting/notebooks/EvaluateJointIndi.ipynb --ckpt=2502/BioSR-joint_indi-l1/5 --mixing_t_ood=0.5  --ckpt_time_predictor=2502/BioSR-UnetClassifier-l2/4 --outputdir=/group/jug/ashesh/indiSplit/notebook_results/ --training_rootdir=PATH_TO_WHERE_ALL_MODELS_ARE_SAVED
```

### Directly Imaged Inputs
```
python notebooks/evaluate_notebook.py --enable_real_input=true --ckpt=2502/HT_LIF24-joint_indi-l1/60 --mmse_count=10  --notebook=/home/ashesh.ashesh/code/DiffSplitting/notebooks/EvaluateJointIndiRealInput.ipynb --ckpt_time_predictor=2502/HT_LIF24-UnetClassifier-l2/3 --training_rootdir=PATH_TO_WHERE_ALL_MODELS_ARE_SAVED
```


## Datasets
The publicly available datasets were utilized in our study. We adopted the same train/validation/test splits as those employed by previous methods focused on semantic unmixing. For convenience and clarity, we also provide these train/validation/test splits here.

All datasets are available on [Zenodo](https://doi.org/10.5281/zenodo.17752782).

The scSplit dataset were adapted from the following previously-published datasets:
- [Hagen et al.](https://gigadb.org/dataset/100888)
- [BioSR](https://figshare.com/articles/dataset/BioSR/13264793)
- [HTT24](https://download.fht.org/jug/msplit/ht_t24/data/ht_t24.zip)
- [HTLIF24](https://download.fht.org/jug/msplit/ht_lif24/data/ht_lif24_500ms.zip)
- [PaviaATN](https://zenodo.org/records/8235843)

## Pre-trained models
- [Hagen et al.](https://bioimage.io/#/artifacts/stupendous-swan)
- [BioSR](https://bioimage.io/#/artifacts/handsome-sloth)
- [HTT24](https://bioimage.io/#/artifacts/faithful-swan)
- [HTLIF24](https://bioimage.io/#/artifacts/amiable-otter)
- [PaviaATN](https://bioimage.io/#/artifacts/kind-badger)


## For evaluating other scSplit variants. 
In addition to evaluating scSplit, the same checkpoints can be used to evaluate other scSplit variants mentioned in the paper. One needs to make following changes to the arguments while evaluating the notebooks: 

- For $\text{scSplit}_{0.5}$, provide `--infer_time=False --use_aggregated_inferred_time=False --use_hardcoded_time_for_inference=0.5` and donot provide `ckpt_time_predictor`. 
- For $\text{scSplit}_{-agg}$, provide ` --use_aggregated_inferred_time=False`.
`
