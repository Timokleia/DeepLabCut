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

**Tips for Calibration Images**:
- Use a large checkerboard with at least 8x6 squares.
- Take at least 70 pairs of images covering various distances and angles.
- Maintain consistent orientation of the checkerboard.
- Ensure the images cover all parts of the camera's view.

### Calibration and Triangulation

After setting up your project, the next steps involve calibrating the cameras and then using triangulation to analyze the 3D movements. This involves using the calibration images to ensure that the cameras are accurately capturing the 3D space.



