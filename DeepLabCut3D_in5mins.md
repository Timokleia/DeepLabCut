###3D DeepLabCut in 5 minutes
##What is 3D DeepLabCut?

DeepLabCut (DLC) is a tool used for estimating the pose or position of objects or animals in videos. The 3D version of DeepLabCut allows you to analyze videos from multiple camera angles to create a three-dimensional view.

Key Points:

Two-Camera System: The standard setup in this guide uses two cameras for creating 3D images. This is known as a 'stereo configuration'. 
It's like using two eyes to get a depth perception of an object.

Preparing for 3D Analysis:
1. You need videos that you want to analyze. These videos should already be processed with DeepLabCut in 2D.
The system works best with videos from two cameras, but you can also use videos from more cameras.
2. You must have calibration images. These are special images used to help the system understand how your cameras are set up.

Options for More than Two Cameras:
If you need to use more than two cameras, there are other tools that work with DeepLabCut:
- AcinoSet: Good for working with many cameras. It's especially tailored for studying cheetahs, so some extra coding might be needed for other uses.
- Anipose: This is another tool that supports DeepLabCut with more than three cameras.
- Argus, easywand, or DLTdv with DeepLabCut: These are additional tools that can be used for more complex setups.
