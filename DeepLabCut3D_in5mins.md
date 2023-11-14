# 3D DeepLabCut in 5 minutes


## What is 3D DeepLabCut?

The 3D version of DeepLabCut allows you to analyze videos from multiple camera angles to create a three-dimensional view.

**Key Points**:

**Two-Camera System**: The standard setup in this guide uses two cameras for creating 3D images. This is known as a 'stereo configuration'. 
It's like using two eyes to get a depth perception of an object.

**Preparing for 3D Analysis**:
1. You need videos that you want to analyze. These videos should already be processed with DeepLabCut in 2D.
The system works best with videos from two cameras, but you can also use videos from more cameras.
2. You must have calibration images. These are special images used to help the system understand how your cameras are set up.

## Getting Started with 3D Projects in DeepLabCut

DeepLabCut supports both single animal and multi-animal (maDLC) projects in 3D. The process involves using two cameras to create a 3D view of the animal's movements.

## Steps to Create a New 3D Project

### Setting Up Your Project:
Watch a [DEMO VIDEO](https://youtu.be/Eh6oIGE4dwI) on how to use this code, and check out the Notebook [here](https://github.com/DeepLabCut/DeepLabCut/blob/master/examples/JUPYTER/Demo_3D_DeepLabCut.ipynb)!

### Creating the Project:

Use the create_new_project_3d function in DeepLabCut.
This function sets up a new project directory and needed files.
You'll need to provide a project name, your name (as the labeler), and specify that you are using 2 cameras.

Here's the command you would use in Python:
```python
deeplabcut.create_new_project_3d('ProjectName','NameofLabeler',num_cameras = 2)
```
You can choose where to save the project by specifying a working_directory: ``working_directory=`Full path of the working directory'`` .
Optionally, create a variable for easy reference to your project's configuration file ``config_path3d=deeplabcut.create_new_project_3d(...``.

### Understanding the Project Directory:
Your project will have several subdirectories for different purposes:
- **calibration_images**: Stores images used to calibrate the cameras. These are images of a checkerboard taken from different angles and distances by both cameras.
- **camera_matrix**: Contains camera parameters that help in transforming 3D coordinates to 2D image coordinates.
corners: Saves patterns detected in calibration images, important for camera calibration.
- **corners:**  As a part of camera calibration, the checkerboard pattern is detected in the calibration images and these patterns will be stored in this directory. Each row of the checkerboard grid is marked with a unique color.
- **undistortion**: Used for checking calibration accuracy. It has images and points that have been corrected for any distortion.

### Calibration and Triangulation

Camera calibration is crucial for accurate 3D pose estimation in DeepLabCut. This process involves using images of a checkerboard taken from different angles and distances to help the system understand how the cameras see the 3D space.

**Steps for Camera Calibration:**

1. Preparing the Checkerboard
- Use a printed checkerboard for calibration. You can find examples here https://markhedleyjones.com/projects/calibration-checkerboard-collection.
- Mount the checkerboard on a flat, hard surface to keep it stable.
- Keep the orientation of the checkerboard consistent and avoid rotating it more than 30 degrees.
- Cover different distances and make sure to capture all parts of the camera’s view.
- Aim to take 30-70 pairs of images. Some images may be discarded later if the checkerboard corners are not detected correctly.
Using Videos for Calibration:
If you prefer, you can record a video moving the checkerboard around and then extract frames from it as .jpg images.
Use this command to convert video to images (modify as needed):
```python
ffmpeg -i videoname.mp4 -vframes 20 camera-1-%03d.jpg
```

2. Processing the Images:
- Place your images in the calibration_images directory of your project.
- Run the deeplabcut.calibrate_cameras function to start the calibration process. This function will try to detect the checkerboard pattern in your images.
```python
deeplabcut.calibrate_cameras(config_path3d, cbrow=8, cbcol=6, calibrate=False, alpha=0.9)
```
- If the corners are not detected correctly in some images, you need to remove those images from the calibration_images folder.
Configuring and Running Calibration:
Edit the config.yaml file to set the camera names and ensure they remain consistent.
Use the following command to calibrate the cameras (modify cbrow and cbcol based on your checkerboard):
