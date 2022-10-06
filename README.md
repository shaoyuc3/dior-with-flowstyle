# Dressing in Order (DiOr) on VITON Dataset
This is an implementation of __Dressing in Order__ on VITON dataset using __Parser-Based Appearance Flow Style__.

![](Images/try_on_example.png)

__More results__

Play with [demo.ipynb](demo.ipynb)!


## Requirements
- python 3.6.13
- torch 1.10.1
- torchvision 0.11.2
- opencv
- visdom


## Get Started
Run
```
pip install -r requirements.txt
```

__Note__: If one wants to run inference only, please specify ```--frozen_flownet```.


## Dataset
Download the __VITON Dataset__ from [here](https://drive.google.com/file/d/11kKsTXoRwfMzx32I6OADJYPmlRLxpRv8/view?usp=sharing).

### Custom dataset/from scratch
For custom dataset or train from scratch, please generate the data folder with the same structure as below.

1. human parse: [Human Parser](https://github.com/GoGoDuck912/Self-Correction-Human-Parsing) (with LIP labels)
2. pose: [Openpose](https://github.com/ZheC/Realtime_Multi-Person_Pose_Estimation)
3. dense pose: [DensePose](https://github.com/facebookresearch/DensePose)


## Run Demo
You can download the pretrained weights from [here](https://drive.google.com/drive/folders/1Qgaj4n9e412CDdNQOWiuIuEy8Ysx2HrY) and unzip it at ```checkpoints/```.

After downloading the pretrained model and setting the data, you can try out the applications in notebook [demo.ipynb](demo.ipynb).


## Training
__Parser-Based Appearance Flow Style__

To warm up the Parser-Based Appearance Flow Field Estimator, first download the vgg checkpoint from [here](https://github.com/senhe/flow-style-vton) and put the checkpoint under the folder ```models/networks/flowStyle```.

Then run
```
sh scripts/run_PBAFN.sh
```

__Note__: if you don't want to warmup the Estimator, you can extract the weights from [here](https://drive.google.com/drive/folders/1upRRswJf_hXldl48w5QCX7LOJp71otJB).

__Training__

After warming up the flownet, train the pipeline by running
```
sh scripts/run_train.sh
```

## Evaluations
To evaluate (SSIM, FID and LPIPS) and generate images, run
```
sh scripts/run_eval.sh
```
__Note__: Please change ```test_pairs.txt``` to ```test_pairs_same.txt``` in the code [here](https://github.com/shaoyuc3/dior-VITON/blob/28a2f7d6b59ada603e3786957863901509af344b/datasets/viton_datasets.py#L51) and [here](https://github.com/shaoyuc3/dior-VITON/blob/28a2f7d6b59ada603e3786957863901509af344b/datasets/viton_datasets.py#L130) when calculating SSIM and LPIPS.

Our generated images [\[256x192\]](https://drive.google.com/drive/folders/16RHLVxGx7kYD_YZ7MoSOlWO2TnHHXYVU).
Our evaluation compared with other try-on methods on VITON dataset is as below.
|         | SSIM(&#8593;)  | FID(&#8595;) |
| ------------- | ------------:| ------------:|
| [VTON](https://github.com/xthan/VITON)        | 0.74  | 55.71 |
| [CP-VTON](https://github.com/sergeywong/cp-vton)        | 0.72  | 24.45 |
| [Cloth-flow](https://openaccess.thecvf.com/content_ICCV_2019/papers/Han_FiNet_Compatible_and_Diverse_Fashion_Image_Inpainting_ICCV_2019_paper.pdf)        | 0.84  | 14.43 |
| [OVNET](https://paperswithcode.com/paper/toward-accurate-and-realistic-outfits)        | 0.85  | - |
| [FlowStyle (Parse-based)](https://github.com/senhe/flow-style-vton)        | 0.82  | 12.11 |
| [SDAFN](https://github.com/OFA-Sys/DAFlow)        | 0.89  | 12.05 |
| DiOr-VITON (ours)        | 0.81 | 13.70 |



## Acknowledgements
This repository is based on [DiOr](https://github.com/cuiaiyu/dressing-in-order) and [Flow-Style-VTON](https://github.com/SenHe/Flow-Style-VTON).
