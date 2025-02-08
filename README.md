# Multi-Domain Balanced Sampling
This repository is the official implementation of the paper [Multi-Domain Balanced Sampling Improves Out-of-Distribution Generalization of Chest X-ray Pathology Prediction Models](http://www.cse.cuhk.edu.hk/~qdou/public/medneurips2021/77_chest_ood_med_neurips_2021.pdf) accepted at [Medical Imaging meets NeurIPS 2021](https://sites.google.com/view/med-neurips-2021) (Med-NeurIPS 2021). \
The arXiv version can be found [here](https://arxiv.org/abs/2112.13734).


## September 2022 Updates - Quantization Aware Training
We have added the script `chest_quantize.py` to allow users run the experiment on a CPU with a smaller model size. For now, only ResNet-50 is supported. \
One can train a quantized model by running:
```
python chest_quantize.py --dataset_dir <data dir> --train_datas cx pc --val_data mc --seed 0
```
Test the model by running:
```
python chest_quantize.py --dataset_dir <data dir> --train_datas cx pc --val_data mc --seed 0 --test_only --test_data nih
```


## Four (4) Pathologies, Four (4) Datasets, & Leave-One-out Cross-Validation
There are 12 different training, validation and test settings generated by combining 4 different Chest X-ray datasets (NIH ChestX-ray8 dataset, PadChest dataset, CheXpert, and MIMIC-CXR). 
The dataset names are condensed as short strings: `"nih"`= NIH ChestX-ray8 dataset, `"pc"` = PadChest dataset, `"cx"` = CheXpert, and `"mc"` = MIMIC-CXR. \
The models used in this experiment are: DenseNet-121 and ResNet-50. \
For each experiment, we compute the ROC-AUC for the following chest x-ray pathologies (labels): Cardiomegaly, Effusion, Edema, Consolidation.

For each experiment, train on two (2) datasets, validate on one (1) leave-out set and test on the remaining one (1) leave-out set. \
The [chest.py](https://github.com/etetteh/OoD_Gen-Chest_Xray/blob/main/chest.py) file contains code to run the models in this study.


### Train Using Baseline Model (Merged Datasets)
To train a DenseNet-121 model with the **Baseline** approach, on CheXpert and PadChest, and validate on the MIMIC-CXR dataset, with seed=0 run the following code:
```
python chest.py --merge_train --arch densenet121 --train_datas cx pc --val_data mc --seed 0
```

### Train Balanced Mini-Batch Sampling
To train a DenseNet-121 model with the **Balanced Mini-Batch Sampling** strategy on CheXpert and PadChest, and validate on the MIMIC-CXR dataset, with seed=0 run the following code:
```
python chest.py --arch densenet121 --train_datas cx pc --val_data mc --seed 0 
```

### Inference
To run inference, add the arguments `--test_only` and `test_data` to the same code you pass for training. \
Example: running inference on NIH dataset
```
python chest.py --arch densenet121 --train_datas cx pc --val_data mc --seed 0 --test_only --test_data nih
```

### Inference using the XRV model
To perform inference using the DenseNet model with pretrained weights from [torchxrayvision](https://github.com/mlmed/torchxrayvision), run the following line of code:
```
python xrv_test.py --test_data pc --seed 0
```
Note that you can pass any of the arguments `pc`, `mc`, `cx` or `nih` to `--test_data` to run inference on PadChest, MIMIC-CXR, CheXpert and ChestX-Ray8 respectively. 


## Requirements (Installations)
In your terminal run `pip install scikit-learn wandb torch torchvision torchxrayvision`
or 
```
git clone https://github.com/etetteh/OoD_Gen-Chest_Xray.git
cd OoD_Gen-Chest_Xray
pip install -r requirements.txt
```


## Citation
```
@inproceedings{tetteh2020balanced,
  title={Multi-Domain Balanced Sampling Improves Out-of-Distribution Generalization of Chest X-ray Pathology Prediction Models},
  author={Tetteh, Enoch and Viviano, Joseph and Bengio, Yoshua and Krueger, David and Cohen, Joseph Paul},
  booktitle={Medical Imaging Meets NeurIPS},
  publisher = {arXiv},
  year={2021},
  url={https://arxiv.org/abs/2112.13734}
}
```

