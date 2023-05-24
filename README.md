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

In order to have the versatility to use unreal_airsim, UE 4.25 is the recommended version. For UE4 to be installed on Linux, you need to register with Epic Games, and build it from source. Please follow detailed instructions on their [website](https://qhfang.github.io/)[website] (https://docs.unrealengine.com/4.27/en-US/SharingAndReleasing/Linux/BeginnerLinuxDeveloper/SettingUpAnUnrealWorkflow/) to set everything up. If you plan to use only pre-compiled binaries as simulation worlds, this section can be omitted,

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

Change path/to/AccurateACL/ and /path/to/Dataset/ to the local directories in which AccurateACL is installed and the Dataset is downloaded.

```bash
docker run --rm --runtime=nvidia --gpus all -it -v /path/to/AccurateACL/:/local/AccurateACL -v /path/to/Dataset/:/local/ACL-Synthetic qihfang/spaint_python3.6.8_cuda10.0_torch1.9.0_spaint:v1.1 bash
```
Note: the above command assumes the ACL-Synthetic dataset is downloaded

### Install packages within container

Navigate to AccurateACL directory.

```bash
pip install -r requirements.txt 
```

#### Usage

For the policy_test.py script to run successfully, the UE4 Editor must first be ran (instruction for how to do so can be found in the same link as that for the installation of UE4), and once the appropriate project is opened, the 'play' button should be pressed.

```bash
python test/policy_test.py --exp-name=test --scene-name=C03 --seq-name=seq-50cm-60deg --net-type=uncertainty --env=EnvRelocUncertainty-v0 --ckpt=ckpt/pretrained_model.pkl --cfg=configs/test.yaml --cuda-idx=0 --vistxt
```
## Loading scene into Unreal Engine

The following video provides a concrete example of importing 3rd party environments in Unreal Engine for AirSim: https://www.youtube.com/watch?v=oR-LXzWENYg. A mesh can be imported for each of the scenes in the Dataset (for example: /ACL-Synthetic/C03/mesh/scene_mesh_lowres.obj). 

### Scene scaling and coordinate system alignment

The scene should be scaled by 100 along each axis, and the transformation variables (translation and rotation) should be set to 0. This should lead to the global coordinate system in UE4 and the local coordinate system attached to the imported mesh coinciding in position. Unreal Engine uses a left handed coordinate system, and AirSim along with AccurateACL use a right handed coordinate system. Additionally, these three coordinate systems are rotated relative to eachother and rotations are expressed in different ways for each. Accommodations for these differences were made in the code. Additionally, the above scaling will lead to 1 unit in Unreal Engine corresponding to 1 meter - the units AccurateACL uses. 

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

