## Vehicle Detection and Tracking
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

This repo contains the written code to complete the project **Vehicle Detection and Tracking** on Udacity Self-Driving Car Nanodegree. The goal is to detect lane lines from images (and video) taken from a camera at the front of a car.

Prerequisites
---
To run this project, it is necessary to have [Anaconda 4.3.30](https://anaconda.org/conda-canary/conda/files?version=4.3.30) installed.

Installation
---
First, clone the repository:
```
git clone https://github.com/shohne/CarND-Vehicle-Detection-Tracking.git
```
Change current directory:
```
cd CarND-Advanced-Lane-Lines
```
Create a conda environment with all dependencies:
```
conda env create -f environment.yml
```
The name of the created environment is *vehicle-detection*.

Running the Notebook
---
Activate the created conda environment:
```
source activate lane-advanced
```
And run Jupyter Notebook:
```
jupyter notebook P5.ipynb
```
Implementation Details
---
Please visit the [report.md](report.md) for more information about the algorithm pipeline and its shortcomings.

Ouput and List of Files
---
As an example of the produced video running the iPython Notebook:

[project_video.mp4](output/project_video.mp4)

Some useful files and folders in this project:

- **P5.ipynb** iPython Notebook with the implementation;
- **environment.yml** used to create conda environment;
- **report.md** detailed description of pipeline;
