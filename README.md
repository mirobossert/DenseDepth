## [High Quality Monocular Depth Estimation via Transfer Learning (arXiv 2018)](https://arxiv.org/abs/1812.11941)
**[Ibraheem Alhashim](https://ialhashim.github.io/)** and **Peter Wonka**

Offical Keras (TensorFlow) implementaiton. If you have any questions or need more help with the code, feel free to contact the first author.

**[Modifications Start]**

For a school project I tried to convert this model to work with TensorFlow.js. It didn't work out of the box because of the custom Layer 'BilinearUpSampling2D' so i tried to replace it with 'UpSampling2D' which is supported by TF.js, then retrain the model and finally convert it to the format needed by TF.js.

I used the cloud service "paperspace" with a P6000 machine for training. The steps I followed/things i learned:
* The downloading of the dataset directly to the remote machine was a bit complicated because it was hosted on GoogleDrive. This workaround helped to download the file: https://unix.stackexchange.com/a/332277
* To use the GPU for training the conda env had to be set up as follows ([TF build configurations](https://www.tensorflow.org/install/source#tested_build_configurations)):

  ```shell
  conda create -n densedepth pip python=3.7 cudnn cupti cudatoolkit=10.0

  conda activate densedepth

  pip install tensorflow-gpu==1.13.1 keras pillow matplotlib scikit-learn scikit-image opencv-python pydot
  ```

* To prevent the training process from stopping on the remote machine I used the very useful `screen` utility.
* Now I was able to train the Model but upon saving the .h5 file to the disk an error occured: `TypeError('Not JSON Serializable...`. After the fixes explained here it finally worked: https://github.com/keras-team/keras/issues/9342#issuecomment-396056333
* After training the model for one epoch I tried to convert it into a TensorFlow.js and tried to run it in the browser. Unfortunatly i wasn't able to run it. As it appeared there was still an issue with the custom layers implemented in keras which were not compatible with TF.js so we decided to use build our project using the RunwayML Network API.

**[Modifications End]**



**[Update]** Added a Colab notebook to try the method on the fly.

**[Update]** Experimental TensorFlow 2.0 implementation added.

**[Update]** Experimental PyTorch code added.

## Results

* KITTI
<p align="center"><img style="max-width:500px" src="https://s3-eu-west-1.amazonaws.com/densedepth/densedepth_results_01.jpg" alt="KITTI"></p>

* NYU Depth V2
<p align="center">
  <img style="max-width:500px" src="https://s3-eu-west-1.amazonaws.com/densedepth/densedepth_results_02.jpg" alt="NYU Depth v2">
  <img style="max-width:500px" src="https://s3-eu-west-1.amazonaws.com/densedepth/densedepth_results_03.jpg" alt="NYU Depth v2 table">
</p>

## Requirements
* This code is tested with Keras 2.2.4, Tensorflow 1.13, CUDA 9.0, on a machine with an NVIDIA Titan V and 16GB+ RAM running on Windows 10 or Ubuntu 16.
* Other packages needed `keras pillow matplotlib scikit-learn scikit-image opencv-python pydot` and `GraphViz` for the model graph visualization and `PyGLM PySide2 pyopengl` for the GUI demo.
* Minimum hardware tested on for inference NVIDIA GeForce 940MX (laptop) / NVIDIA GeForce GTX 950 (desktop).
* Training takes about 24 hours on a single NVIDIA TITAN RTX with batch size 8.

## Pre-trained Models
* [NYU Depth V2](https://s3-eu-west-1.amazonaws.com/densedepth/nyu.h5) (165 MB)
* [KITTI](https://s3-eu-west-1.amazonaws.com/densedepth/kitti.h5) (165 MB)

## Demos
* After downloading the pre-trained model (nyu.h5), run `python test.py`. You should see a montage of images with their estimated depth maps.
* **[Update]** A Qt demo showing 3D point clouds from the webcam or an image. Simply run `python demo.py`. It requires the packages `PyGLM PySide2 pyopengl`.
<p align="center">
  <img style="max-width:500px" src="https://s3-eu-west-1.amazonaws.com/densedepth/densedepth_results_04.jpg" alt="RGBD Demo">
</p>

## Data
* [NYU Depth V2 (50K)](https://tinyurl.com/nyu-data-zip) (4.1 GB): You don't need to extract the dataset since the code loads the entire zip file into memory when training.
* [KITTI](http://www.cvlibs.net/datasets/kitti/): copy the raw data to a folder with the path '../kitti'. Our method expects dense input depth maps, therefore, you need to run a depth [inpainting method](https://cs.nyu.edu/~silberman/datasets/nyu_depth_v2.html) on the Lidar data. For our experiments, we used our [Python re-implmentaiton](https://gist.github.com/ialhashim/be6235489a9c43c6d240e8331836586a) of the Matlab code provided with NYU Depth V2 toolbox. The entire 80K images took 2 hours on an 80 nodes cluster for inpainting. For our training, we used the subset defined [here](https://s3-eu-west-1.amazonaws.com/densedepth/kitti_train.csv).
* [Unreal-1k](https://github.com/ialhashim/DenseDepth): coming soon.

## Training
* Run `python train.py --data nyu --gpus 4 --bs 8`.

## Evaluation
* Download, but don't extract, the ground truth test data from [here](https://s3-eu-west-1.amazonaws.com/densedepth/nyu_test.zip) (1.4 GB). Then simply run `python evaluate.py`.

## Reference
Corresponding paper to cite:
```
@article{Alhashim2018,
  author    = {Ibraheem Alhashim and Peter Wonka},
  title     = {High Quality Monocular Depth Estimation via Transfer Learning},
  journal   = {arXiv e-prints},
  volume    = {abs/1812.11941},
  year      = {2018},
  url       = {https://arxiv.org/abs/1812.11941},
  eid       = {arXiv:1812.11941},
  eprint    = {1812.11941}
}
```
