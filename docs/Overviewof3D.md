(3D-overview)=
# 3D DeepLabCut

In this repo we directly support 2-camera based 3D pose estimation. If you want n camera support, plus nicer optimization methods, please see our new work that was published at [ICRA 2021 on strong baseline 3D models (and a 3D dataset)](https://github.com/African-Robotics-Unit/AcinoSet). In the link you will find how we optimize 6+ camera DLC output data for cheetahs (and see more below).

<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1589578632599-HQENUYUIBI9KYTZA2WXV/ke17ZwdGBToddI8pDm48kBgERiRoVg6XJpnbAnG076FZw-zPPgdn4jUwVcJE1ZvWhcwhEtWJXoshNdA9f1qD7Y5_KuY_fkOEvGrDVB8aRb13EC_7Ld97nVeJG4MMJk1tqSdWG3KOMGCA68a4XjyT5g/3D.png?format=300w" width="350" title="DLC-3D" alt="DLC 3D" align="right" vspace = "50">


## **Prerequisites for 3D Analysis**

**Our code base in this repo assumes:**

A. A. You have 2D videos and have employed a DeepLabCut network for their analysis as described in the [main documentation](overview). 
    The analysis has used either:
        - Multiple, independent networks for each camera (though this is less recommended), or
        - A single network trained across all views (**recommended**! Reference: [Nath*, Mathis* et al., 2019](https://www.biorxiv.org/content/10.1101/476531v1)).
        - Multi-animal 3D analysis is also supported by this code (please see [Lauer et al. 2022](https://doi.org/10.1038/s41592-022-01443-0)).


B. You are using 2 cameras, in a [stereo configuration](https://github.com/DeepLabCut/DeepLabCut/blob/5ac4c8cb6bcf2314a3abfcf979b8dd170608e094/deeplabcut/pose_estimation_3d/camera_calibration.py#L223), for 3D*.

C. You have captured calibration images (further details in the sections below!).


### ***If you need more than 2 camera support:***
Here are other excellent options for you to use that extend DeepLabCut:

<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1628432165795-BBF6AWCK1BEKV3AJ6GF5/cheetah.gif?format=1500w" width="350" title="AcinoSet-3D" alt="DLC 3D" align="right" vspace = "50">

- **[AcinoSet](https://github.com/African-Robotics-Unit/AcinoSet)**; **n**-camera support with triangulation, extended Kalman filtering, and trajectory optimization code (see video to the right for a min demo, courtesy of Prof. Patel), plus a GUI to visualize 3D data. It is built to work directly with DeepLabCut (but currently tailored to cheetah's, thus some coding skills are required at this time).


- **[anipose.org](https://anipose.readthedocs.io/en/latest/)**; a wrapper for 3D deeplabcut that provides >3 camera support and is built to work directly with DeepLabCut. You can `pip install anipose` into your DLC conda environment.

- **Argus, easywand or DLTdv** w/DeepLabCut see https://github.com/haliaetus13/DLCconverterDLT; this can be used with the the highly popular Argus or DLTdv tools for wand calibration.

## Jump in with direct DeepLabCut 2-camera support🔥:

- single animal DeepLabCut and multi-animal DeepLabCut (maDLC) projects are supported:

<p align="center">
<img src= "https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1560968522350-COKR986AQESF5N1N7QNK/ke17ZwdGBToddI8pDm48kNaO57GzHjWqV-xM6jVvY6ZZw-zPPgdn4jUwVcJE1ZvWQUxwkmyExglNqGp0IvTJZUJFbgE-7XRK3dMEBRBhUpyR5k0u27ivMv3az5DOhUvLuYQefjfUWYPEDVexVC_mSas4X78tjQKn3yE00zHvnK8/3D_maousLarger.gif?format=750w" height="200">

<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/cd423302-0389-4b63-8869-b787a2c52b8b/maDLC_3d.gif?format=1500w" height="200">
</p>

### (1) Create a New 3D Project:

🎥[DEMO VIDEO](https://youtu.be/Eh6oIGE4dwI)|📘[Notebook Guide](https://github.com/DeepLabCut/DeepLabCut/blob/master/examples/JUPYTER/Demo_3D_DeepLabCut.ipynb)!

**In a Nutshell:**

- **Create once**: Run **`create_new_project_3d`** once per project (defined by a set of cameras and calibration images).
- The function **`create_new_project_3d`** creates a new project directory specifically for converting the 2D pose to 3D pose, required subdirectories, and a basic 3D project configuration file.
- **Name it**: Projects are identified by the `'ProjectName'` (name of the project, e.g. Project1), `'NameofExperimenter'`(name of the experimenter, e.g. YourName), and creation date.

This function requires the user to input `'ProjectName'` (the name of the project),`'NameofExperimenter'` (the name of the experimenter)  and `num_cameras=2` (the number of cameras to be used).

To start a 3D project type the following in ipython 🐍:

```python
deeplabcut.create_new_project_3d('ProjectName','NameofLabeler',num_cameras = 2)
```
Currently, DeepLabCut supports triangulation using 2 cameras, but will expand to more than 2 cameras in a future version.

**TIPS🪄:**

1. Specify a different folder with **`working_directory='Full path of the working directory'`** if you want to place this folder somewhere beside the current directory you are working in.
2. You can place `config_path3d` in front of `deeplabcut.create_new_project_3d` to create a variable that holds the path to the config.yaml file.

```python
config_path3d = deeplabcut.create_new_project_3d('ProjectName', 'NameofExperimenter', num_cameras=2)
```
Or, set **`config_path3d = 'Full path of the 3D project configuration file'`**.

**What You Get:**

- **Project Directory**: Named as 'ProjectName_ExperimenterName_CreationDate_3d' in the working directory.
- **Subdirectories**: **`calibration_images`**, **`camera_matrix`**, **`corners`**, and **`undistortion`** —each housing respective data and outputs.

**Subdirectory Files💡:**

- **calibration_images**: Stores pairs of calibration images from the two cameras, saved as **`.jpg`** (e.g., **`camera-1-01.jpg`** and **`camera-2-01.jpg`**). A calibration image can be acquired using a printed checkerboard and its pair wise images are taken from both cameras to consider as a set of calibration images. Ensure the checkerboard is visible and stable across various distances and orientations (more guidelines below).
- **camera_matrix**: Saves camera parameters (intrinsic and extrinsic) as pickle files that store the intrinsic and extrinsic camera parameters. The intrinsic parameters represent a transformation from 3-D camera’s coordinates into the image coordinates, while the extrinsic parameters represent a rigid transformation from world coordinate system to the 3-D camera’s coordinate system.
- **corners**: Contains detected checkerboard patterns from calibration images.
- **undistortion**: Houses undistorted calibration images overlaid with points for verification.

 Here is an overview of the calibration and triangulation workflow that follows:

 <p align="center">
<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1559751031211-IOTHQDAEEFP939AD8L8Q/ke17ZwdGBToddI8pDm48kCpBvlJgRextwO-RLKSiThBZw-zPPgdn4jUwVcJE1ZvWQUxwkmyExglNqGp0IvTJZamWLI2zvYWH8K3-s_4yszcp2ryTI0HqTOaaUohrI8PIMRcHFn6kFHmdB2CIEK_nkzQWBtRKl1IphR3INrGSAiA/3dworkflow.png?format=1000w" width="55%">
</p>

### (2) Take and Process Camera Calibration Images:
### A. Capture Checkerboard Images 📸

**CRITICAL!** Use checkerboard images for calibration. Consider printing and using boards from [this collection](https://markhedleyjones.com/projects/calibration-checkerboard-collection), ensuring they are mounted on a flat, hard surface.
- Image pairs must be stored as **`.jpg`** files.
- Ensure names are prefixed with camera-#, i.e., **`camera-1-01.jpg`** and **`camera-2-01.jpg`** for the initial image pair. Note: Name alterations are not permitted post-project creation.

**Tip:** To convert a short video into **`.jpg`** frames using the first 20 frames, run the following command in your conda environment:

```python
ffmpeg -i videoname.mp4 -vframes 20 camera-1-%03d.jpg
```

### B. Guidelines for Capturing Images 🧐

- Consistently maintain the checkerboard’s orientation and limit rotation to under 30 degrees.
- Vary distances, ensuring every image view (corners and center) is covered at each distance.
- Employ a sizable checkerboard, optimally with at least 8x6 squares.
- Capture at least 30-70 pairs of images as some may be discarded due to wrong corner detection.

### C. Calibration Iterative Process 🔄

Calibration, an iterative process, requires accurate grid pattern detection from calibration images, managed through **`deeplabcut.calibrate_cameras(config_path)`**. The grid pattern could be of various sizes, such as 8x8 or 5x5. Here, an 8x6 grid pattern is utilized to detect the checkerboard’s internal corners.

<p align="center">
<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1559776966423-RATM6ZQT8JXHYAN768F6/ke17ZwdGBToddI8pDm48kKmw982fUOZVIQXHUCR1F55Zw-zPPgdn4jUwVcJE1ZvWQUxwkmyExglNqGp0IvTJZUJFbgE-7XRK3dMEBRBhUpw5XnxLBmEFHJGf_0qFdDpmIncOw4kq9OpCHNTYqzGO-E1YJr-Thht9Tdog4YtCwrE/right02_corner.jpg?format=500w" height="220">
 <img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1559776952829-KRHFX74CDO3BPIY9E9U0/ke17ZwdGBToddI8pDm48kKmw982fUOZVIQXHUCR1F55Zw-zPPgdn4jUwVcJE1ZvWQUxwkmyExglNqGp0IvTJZUJFbgE-7XRK3dMEBRBhUpw5XnxLBmEFHJGf_0qFdDpmIncOw4kq9OpCHNTYqzGO-E1YJr-Thht9Tdog4YtCwrE/left02_corner.jpg?format=500w" height="220">
</p>

Faulty corner detections or incorrect corner orders between the images from camera-1 and camera-2 require you to eliminate those image pairs from the **`calibration_images`** folder to maintain calibration accuracy.

1. Begin by placing your images into the **`calibration_images`** directory.

**CRITICAL!** Adjust the **`config.yaml`** file to adjust camera names, and avoid altering them post-setting!

Run the command:

```python
deeplabcut.calibrate_cameras(config_path3d, cbrow=8, cbcol=6, calibrate=False, alpha=0.9)
```

**NOTE**: Make sure to define your checkerboard’s rows (**`cbrow`**) and columns (**`cbcol`**). Initially, set **`calibrate`** to **`False`** to enable the removal of faulty images. The visual inspection of the output is important to verify detected corners and select image pairs with accurate corner detection. If **`alpha`**=0 (scaling parameter), an undistorted image is returned with a minimum of unwanted pixels, even possibly eliminating some at the image corners. An ** `alpha`** of 1 maintains all pixels but may introduce some additional black images.

After careful selection of image sets—remember, only keep the pairs where the corners and their order are spot on. —cameras are ready for calibration.

Now, let's calibrate those cameras with:

```python
deeplabcut.calibrate_cameras(config_path3d, cbrow=8, cbcol=6, calibrate=True, alpha=0.9)
```
Here’s what's happening💭: the function's figuring out the intrinsic and extrinsic parameters for each camera and estimating the re-projection error using these. This error gives us a sneak peek into how on-point these parameters are. It also estimates the transformation between our two cameras and gets them stereo calibrated. The stereo rectification computed by the function ensures both camera image planes are brought to the same level. All these details get safely stored into a pickle file named **`stereo_params.pickle`** in the **`camera_matrix`** directory.

After performing this function for your project, repeating it isn’t necessary unless a re-calibration is necessary. And if you do decide to recalibrate, good documentation is crucial: write down which videos were analyzed with the “old” versus the “new” calibration images to maintain data integrity and analytical consistency.

### **(3) Check for Undistortion 🔄🔍**

Verifying the accuracy of stereo calibration needs an undistortion check. We need to make sure those calibration images and corner points, once they've been undistorted using camera matrices, align just right when projected on the undistorted images. 

To do this just type:

```python
deeplabcut.check_undistortion(config_path3d, cbrow=8, cbcol=6)
```

Each of your calibration images gets undistorted and saved under the **`undistortion`** directory. You’ll also get a plot that pairs undistorted camera images with its undistorted corner points overlay. Make sure to visually inspect these images, verifying the integrity of the undistortion process.

All the undistorted corner points from your calibration images will be triangulated and presented in a plot for you to check for any undistortion errors. If you find discrepancies or inaccuracies, revise those calibration images, redo the calibration, and repeat this step!

---

### **(4) Triangulation: Take your Data from 2D to 3D 🚀🌐**

If the undistortion is accurate, we can triangulate the pose from our two cameras and get the 3D DeepLabCut coordinates! 

### CRITICAL Notes 🚨

- Your video files need to align with the camera names from your config file. If we’re calling them **`camera-1`** and **`camera-2`** (or another variant), your video filenames need to echo this. Use something like **`rig-1-mouse-day1-camera-1.avi`** and **`rig-1-mouse-day1-camera-2.avi` .**
- To pair them up correctly, aside from the camera name, filenames should be identical as well!

Do you need software to record your vids? [Here's our go-to](https://github.com/AdaptiveMotorControlLab/Camera_Control).

**Size Specifications**: Videos don’t need identical pixel sizes, but they should approximate the size of the calibration images, using the same cameras that you used for calibration.

### Tailoring the 3D Project Config.yaml 🛠️💼

- **Linking to 2D**: Edit your 3D project **`config.yaml`** to show which DeepLabCut projects contain the 2D view information.
- **Consistent Body Part Names**: Make sure to use the same body part names as in the 2D project’s **`config.yaml`**.
- **Picking a Snapshot**: Designate the snapshot to use in the 2D config file (default: last training snapshot = `-**1**`).
- **Naming Your 3D Scorer**: Choose a “scorer 3D” name that it will refer to the project file and feature in future 3D output filenames.
- **Skeleton, Assemble**: You might want to define a “skeleton” too. It just connects points during plotting. You decide which points get “skeletonized”. Others will simply be plotted into the 3D space.

Here is how the `config.yaml` looks with some example inputs:

####TOADDIMAGE

### CRITICAL Notes 🚨

This step will conduct an equivalent of **`analyze_videos`** (in 2D) and apply a median filter to the 2D data—with **`filterpredictions=True`** being the default setting! If your 2D analysis has been done and there’s a filtered output file saved, it’ll grab that by default. If not, your unfiltered 2D analysis files will be taken into account!

Next, share the **`config_path3d`** and the path of the video folder (where all those videos from your two cameras are stored). The triangulation in DeepLabCut happens like this:

```python
deeplabcut.triangulate(config_path3d, '/yourcomputer/fullpath/videofolder', filterpredictions=True/False)
```
⚠️ **NOTE for Windows Users,** your paths should look like this: `r'C:\Users\computername\videofolder'` or `'C:\\Users\\computername\\videofolder'` .

💡 **TIP**: Here are all the parameters you can pass:


###TOADDIMAGE

The triangulated file is now saved in the same directory where your video files are located (or the folder you've designated as its destination)! It's all set for any future analysis. This step can be run at anytime as you collect new videos, and easily added to your automated analysis pipeline, i.e. such as ** replacing**`deeplabcut.triangulate(config_path3d, video_path)` with `deeplabcut.analyze_videos` (as if it’s not analyzed in 2D already, this function will take care of it 😉).

<p align="center">
<img src= https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1559758477126-B9PU1EFA7L7L1I24Z2EH/ke17ZwdGBToddI8pDm48kH6mtUjqMdETiS6k4kEkCoR7gQa3H78H3Y0txjaiv_0fDoOvxcdMmMKkDsyUqMSsMWxHk725yiiHCCLfrh8O1z5QPOohDIaIeljMHgDF5CVlOqpeNLcJ80NK65_fV7S1UQf4d-kVja3vCG3Q_2S8RPAcZTZ9JxgjXkf3-Un9aT84H3bqxw7fF48mhrq5Ulr0Hg/howtouseDLC2d_3d-01.png?format=1000w width="65%">
 </p>

### (5) Visualize your 3D DeepLabCut Videos 👀

To visualize both the 2D videos with tracked points plut the pose in 3D, the user can create a 3D video for selected frames (these are large files, so we advise just looking at a subset of frames).

Specify the configuration file `config_path3d`, path of the triangulated file folder `['triangulated_file_folder'],` and the start and end frame indices `start=50, end=250` to create a 3D labeled video. It's important to note that the *triangulated_file_folder* should contain the recently generated file, suffixed with **yourDLC_3D_scorername.h5**. 

This can be done using:

```python
deeplabcut.create_labeled_video_3d(config_path3d, ['triangulated_file_folder'], start=50, end=250)
```

Helpful Tip 🌟: (Discover more parameters below) You can manipulate the axis of the 3D plot (located on the distant right side),  by modifying the variables: xlim, ylim, zlim, and view. Your checkerboard_3d.png image, created in previous steps, can be referred to for determining the axis ranges.

 <p align="center">
<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1559864026106-B6XQHUDUA8VB6F0FNVBA/ke17ZwdGBToddI8pDm48kKmw982fUOZVIQXHUCR1F55Zw-zPPgdn4jUwVcJE1ZvWQUxwkmyExglNqGp0IvTJZUJFbgE-7XRK3dMEBRBhUpx7krGdD6VO1HGZR3BdeCbrijc_yIxzfnirMo-szZRSL5-VIQGAVcQr6HuuQP1evvE/checkerboard_3d.png?format=750w" width="45%">
</p>

The 'view' parameter adjusts the axes' elevation and azimuth, with default values set to [113, 270]. Users are encouraged to experiment to determine their preferred viewpoint. Upon execution of the function, the video is generated from .png files stored in a temporary directory. Users can preview the first image immediately upon command execution. If the view requires adjustment, hit 'CNTRL+C' to halt the process, modify the values, and re-initiate the command.

**Other optional parameters include:**

###TOADDIMAHGE

### If you use this code:

We kindly ask that you cite [Mathis et al, 2018](https://www.nature.com/articles/s41593-018-0209-y) **&** [Nath*, Mathis*, et al., 2019](https://doi.org/10.1038/s41596-019-0176-0). If you use 3D multi-animal: [Lauer et al. 2022](https://doi.org/10.1038/s41592-022-01443-0).

