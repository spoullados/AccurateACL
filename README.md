# Visualising and Analysing [ECCV 2022] Towards Accurate Active Camera Localization in Unreal Egnine using AirSim

## Installations

### Accurate ACL
```bash
git clone https://github.com/qhFang/AccurateACL.git --recursive
```
### Unreal Engine

For the versatility to use unreal_airsim, UE 4.25 is the recommended version. For UE4 to be installed on Linux, you need to register with Epic Games, and build it from source. Please follow detailed instructions on their [website](https://docs.unrealengine.com/4.27/en-US/SharingAndReleasing/Linux/BeginnerLinuxDeveloper/SettingUpAnUnrealWorkflow/) to set everything up. If you plan to use only pre-compiled binaries as simulation worlds, this section can be omitted,

### Airsim

AirSim can be installed as done for [unreal_airsim](https://github.com/ethz-asl/unreal_airsim).

## Dataset

Detailed descriptions about the datasets and how they can be downloaded are provided in the [AccurateACL](https://github.com/qhFang/AccurateACL) repository.

## Passive localizer (random forest)

The docker image contains a proper environment for the random forest. Run the following command to use the image.
```bash
docker pull qihfang/spaint_python3.6.8_cuda10.0_torch1.9.0_spaint:v1.1
```
Note: for those who want to compile from source, instructions can be found in the gitub repository for [AccurateACL](https://github.com/qhFang/AccurateACL). The image also contains other necessary packages that can be installed from the requirements.txt file in the [AccurateACL](https://github.com/qhFang/AccurateACL) repository.

### Mount local directories and files to your container

```bash
docker run --rm --runtime=nvidia --gpus all -it -v /path/to/AccurateACL/:/local/AccurateACL -v /path/to/Dataset/:/local/ACL-Synthetic qihfang/spaint_python3.6.8_cuda10.0_torch1.9.0_spaint:v1.1 bash
```
Change path/to/AccurateACL/ and /path/to/Dataset/ to your local AccurateACL Dataset directories.

Note: the above command assumes the ACL-Synthetic dataset is downloaded

### Install packages within container

```bash
cd /local/AccurateACL
pip install -r requirements.txt 
```

#### Usage

```bash
python test/policy_test.py --exp-name=test --scene-name=C03 --seq-name=seq-50cm-60deg --net-type=uncertainty --env=EnvRelocUncertainty-v0 --ckpt=ckpt/pretrained_model.pkl --cfg=configs/test.yaml --cuda-idx=0 --vistxt
```
For the policy_test.py script to run successfully, you must first [run the UE4Editor](https://docs.unrealengine.com/4.27/en-US/SharingAndReleasing/Linux/BeginnerLinuxDeveloper/SettingUpAnUnrealWorkflow/), open the appropriate project, and press the 'play' button. Details regarding the creation of the project and loading a scene follow.

## Loading scene into Unreal Engine

Several [examples](https://www.youtube.com/watch?v=oR-LXzWENYg) can be found which illustrate the importing of 3rd party environments in Unreal Engine for AirSim. A mesh can be imported for each of the scenes in the Dataset. For example, the mesh for scene C03 can be found /C03/mesh/scene_mesh_lowres.obj once in the ACL-Synthetic directory. 

### Adjustments

The scene should be scaled by 100 along each axis, and the transformation variables (translation and rotation) should be set to 0. The former leads to 1 unit in Unreal Engine corresponding to 1 meter (the unit of displament used in the code) and the latter to the alignment of the global coordinate system in UE4 and the local coordinate system attached to the imported mesh. Accommodations for differences in coordinate systems and 6-DoF transformation conventions in Unreal Engine, AirSim and AccurateACL are made in policy_test.py. 

## Bibtex for the relevant paper
```bibtex
@article{fang2022towards,
  title={Towards Accurate Active Camera Localization},
  author={Fang, Qihang and Yin, Yingda and Fan, Qingnan and Xia, Fei and Dong, Siyan and Wang, Sheng and Wang, Jue and Guibas, Leonidas and Chen, Baoquan},
  journal={ECCV},
  year={2022}
}
```

## Contact
For any questions, feel free to contact us.

Spyros Poullados: [spoullados@ethz.ch](mailto:spoullados@ethz.ch)

Julia Chen: [jiaqchen@ethz.ch](mailto:jiaqchen@ethz.ch)

Siyan Dong: [siyan.dong@inf.ethz.ch](mailto:siyan.dong@inf.ethz.ch)

[Qihang Fang](https://qhfang.github.io/): [qihfang@gmail.com](mailto:qihfang@gmail.com)


