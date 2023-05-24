# [ECCV 2022] Towards Accurate Active Camera Localization

*[Qihang Fang](https://qhfang.github.io/), *[Yingda Yin](https://yd-yin.github.io/), †[Qingnan Fan](https://fqnchina.github.io/), [Fei Xia](https://fxia22.github.io/), [Siyan Dong](https://scholar.google.com/citations?user=vtZMhssAAAAJ&hl=en/), Sheng Wang, [Jue Wang](https://juewang725.github.io/), [Leonidas Guibas](http://geometry.stanford.edu/member/guibas/index.html), †[Baoquan Chen](http://cfcs.pku.edu.cn/baoquan/)

*Equal contribution; ordered alphabetically | †Corresponding authors | [Video](https://youtu.be/pDMoZ6pjkkQ) | [arXiv](https://arxiv.org/abs/2012.04263)

*Spyros Poullados 

<p align="center">
<img src="./images/turn_and_long.gif" alt="demonstration" width="85%">
</p>
<p align="center">
<img src="./images/4sequences.gif" alt="demonstration" width="100%">
</p>



## Installations

### Accurate ACL
```bash
git clone https://github.com/qhFang/AccurateACL.git --recursive
```
### Unreal Engine

In order to have the versatility to use unreal_airsim, UE 4.25 is the recommended version. For UE4 to be installed on Linux, you need to register with Epic Games, and build it from source. Please follow detailed instructions on their website (https://docs.unrealengine.com/4.27/en-US/SharingAndReleasing/Linux/BeginnerLinuxDeveloper/SettingUpAnUnrealWorkflow/) to set everything up. If you plan to use only pre-compiled binaries as simulation worlds, this section can be omitted,

### Airsim

AirSim can be installed as done for unreal_airsim. Details can be found in the following link: https://github.com/ethz-asl/unreal_airsim

## Dataset
### ACL-Synthetic and ACL-Real
<p>
In the paper, we evaluate our algorithm on the ACL-Synthetic and ACL-Real datasets.

<strong>Some virtualizations of ACL-Synthetic dataset</strong>
<div align="center"><img src="./images/ACL-Synthetic.jpg" alt="demonstration" width="100%"></div>

<strong>Some virtualizations of ACL-Real dataset</strong>
<div align="center"><img src="./images/ACL-Real.jpg" alt="demonstration" width="100%"></div>

The ACL-Synthetic and ACL-Real datasets can be downloaded <a href="https://drive.google.com/file/d/1OIKUeQDfdNuxwyTlXP3KpJyk4p-Nsyjn/view?usp=sharing">here</a>.
</p>

### ACL-Origin
<p>
We further collect 120 high-quality indoor scenes. For each scene, we provide a .max format file which contains 3D models for all the furnitures with textures, and lighting effects, shading, and other 3D design elements.

<strong>Panorama example of ACL-Origin:</strong>
<div align="center"><img src="./images/panorama.jpg" alt="demonstration" width="100%"></div>

<strong>Rendering example of ACL-Origin:</strong>


https://user-images.githubusercontent.com/25958029/208108052-d946b8d2-b430-40b1-a381-11ddda755378.mp4



The ACL-Origin datasets can be downloaded <a href="https://1drv.ms/u/s!Al4DaYqDq4-33STwLq83BAwxM6j2?e=m29Glv">here</a>.
</p>

## Passive localizer (random forest)

The docker image contains a proper environment for the random forest. Run the following command to use the image.
```bash
docker pull qihfang/spaint_python3.6.8_cuda10.0_torch1.9.0_spaint:v1.1
```
Note: for those who want to compile from source, instructions can be found in the gitub repository for AccurateACL

### Mount local directories and files to your container

```bash
docker run --rm --runtime=nvidia --gpus all -it -v /local/home/spoullados/Desktop/:/local/Desktop -v /local/home/spoullados/Downloads/:/local/Downloads qihfang/spaint_python3.6.8_cuda10.0_torch1.9.0_spaint:v1.1 bash
```

### Configure paths
Please specify the paths in `global_setting.py` to your paths.

### Install packages within container

Within the AccurateACL directory mounted in the container:
```bash
pip  install -e extensions/gym-foo -e extensions/rlpyt msgpack-rpc-python 
```
## Loading scene into Unreal Engine

## Usage
### Train
```bash
python train/trainer.py --exp_name=training --env=EnvRelocUncertainty-v0 --net_type=uncertainty \
--snapshot_mode=all --batch_B=5 --batch_T=800 --batch_nn=40 --gpu=0 --gpu_cpp=1 --gpu_render=1 \
--cfg=configs/train.yaml
```
### Test

```bash
python test/policy_test.py \ 
--exp-name=test --scene-name=I43 --seq-name=seq-50cm-60deg \
--net-type=uncertainty --env=EnvRelocUncertainty-v0 --ckpt=ckpt/pretrained_model.pkl \
--cfg=configs/test.yaml --cuda-idx=0
```

## Bibtex
```bibtex
@article{fang2022towards,
  title={Towards Accurate Active Camera Localization},
  author={Fang, Qihang and Yin, Yingda and Fan, Qingnan and Xia, Fei and Dong, Siyan and Wang, Sheng and Wang, Jue and Guibas, Leonidas and Chen, Baoquan},
  journal={ECCV},
  year={2022}
}
```


## Contact
For any questions, feel free to contact the authors.

[Qihang Fang](https://qhfang.github.io/): [qihfang@gmail.com](mailto:qihfang@gmail.com)

[Yingda Yin](https://yd-yin.github.io/): [yingda.yin@gmail.com](mailto:yingda.yin@gmail.com)

[Qingnan Fan](https://fqnchina.github.io/): [fqnchina@gmail.com](mailto:fqnchina@gmail.com)


## Acknowledgments
This work was supported in part by NSFC Projects of International Cooperation and Exchanges (62161146002), NSF grant IIS-1763268, a Vannevar Bush Faculty Fellowship, and a gift from the Amazon Research Awards program.

Our RL framework is based on [RLPYT](https://github.com/astooke/rlpyt) by [Adam Stooke](https://www.linkedin.com/in/adam-stooke-06bb6923/) et al. and the passive relocalizer module is based on [spaint](https://github.com/torrvision/spaint) by [Stuart Golodetz](http://research.gxstudios.net/) et al.

