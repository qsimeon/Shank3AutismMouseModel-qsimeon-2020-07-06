# Shank3AutismMouseModel-qsimeon-2020-07-06 ***

Installation (highlights are what I did)
Quick Anaconda Install for Windows 10, MacOS, Ubuntu!
Please use one (or more) of the supplied Anaconda environments for a fast and easy installation process.
(0) Be sure you have Anaconda 3 installed! https://www.anaconda.com/distribution/, and get familiar with using "cmd" or terminal!
(1) Either go to www.deeplabcut.org (at the bottom of the page) to download the correct environment file or download or 
git clone this repo (in the terminal/cmd program, while in a folder you wish to place DeepLabCut type git clone https://github.com/AlexEMG/DeepLabCut.git).
Now, always in Terminal (or Command Prompt), go to the folder named conda-environments using the command "cd" (which stands for change directory). For example, if you downloaded or cloned the repo onto your Desktop, the command may look like: 
cd C:\Users\YourUserName\Desktop\DeepLabCut\conda-environments
To get the location right, a cool trick is to drag the folder and drop it into Terminal. Alternatively, you can (on Windows) hold SHIFT and right-click > Copy as path, or (on Mac) right-click and while in the menu press the OPTION key to reveal Copy as Pathname.
(2) Now, depending on which file you want to use (if with GPUs, see extra note below!!!), open the program terminal or cmd where you placed the file (i.e. cd conda-environments) and then type:
conda env create -f DLC-CPU.yaml
or
conda env create -f DLC-GPU.yaml
(3) You can now use this environment from anywhere on your computer (i.e. no need to go back into the conda- folder). Just enter your environment by running:
Ubuntu/MacOS: source/conda activate nameoftheenv (i.e. on your Mac: conda activate DLC-CPU)
Windows: activate nameoftheenv (i.e. activate DLC-GPU)
Now you should see (nameofenv) on the left of your teminal screen, i.e. (dlc-macOS-CPU) YourName-MacBook... NOTE: no need to run pip install deeplabcut, as it is already installed!!! :)
However, if you ever want to update your DLC, just run pip install --upgrade deeplabcut once you are inside your env. You can check the version by running import deeplabcut deeplabcut.__version__. Don't be afraid to update, DLC is backwards compatable with your 2.0+ projects and peformance continues to get better and new features are added nearly monthly.
Great, that's it!
Simply run ipython or pythonw (macOS only) to launch the terminal, jupyter notebook to launch a browser session, or ipython/pythonw, import deeplabcut, deeplabcut.launch_dlc() to use our Project Manager GUI! Many more details here!




Overview of Terminal Workflow



In-Depth Instructions
https://github.com/AlexEMG/DeepLabCut/blob/f1f3aac06ca88ad477b3c1e18e64c3cd7381e759/docs/functionDetails.md


GUI Workflow (Best to Use for Novice)
Steps to do on local machine
In a Terminal window, execute the command `pythonw -m deeplabcut`. This launches the DeepLabCut GUI.

Create a new project. Name the project ‘Shank3AutismMouse’. For author, type your MIT Kerberos (e.g. Name of the experimenter: ‘qsimeon’). The date of creation will be automatically appended to the project folder as a suffix. 

Choose the videos you want to use to train the model on. On a Mac, select that you want to copy the video (because symbolic links don’t work on Macs).
I had four mouse behavior videos in "/Users/qsimeon/Dropbox (MIT)/Graybiel Lab UROP/DeepLabCut Project/Videos”. Importantly, preprocessed the original videos from the 2019-08-19 Shank WT KO pilot Dropbox folder using a script (www.dropbox.com/s/kdb9scar7l7eu98/process_vids.mlx?dl=0). 

Convert the .avi videos from the Dropbox into .mp4 using a program like (on Mac) FreeMp4 Converter.

I wrote to extract the middle 5th portion of the video and to convert it to a grayscale version that is easier to track the mouse in. 

Select a directory where you want the directory to be created (e.g. "/Users/qsimeon/Dropbox (MIT)/Graybiel Lab UROP/DeepLabCut Project/”).

The above three steps with the GUI are the equivalent of running the Terminal commands (after you cd into the directory  
`pythonw`
`import deeplabcut`
config_path = 'path-to-your-project/Shank3AutismMouse-author-date/config.yaml'
`deeplabcut.create_new_project('Name of the project','Name of the experimenter', ['Full path of video 1','Full path of video2','Full path of video3'], working_directory='Full path of the working directory',copy_videos=True)`

Use the function `deeplabcut.add_new_videos(config_path, videos=<list of full paths to videos>, copy_videos=False, coords=<list of lists of crop coordinates for each video>)` at any stage of the project to add new videos to the config file and extract their frames.
EXAMPLE:
Pythonw
>>> config_path = '/Users/qsimeon/Dropbox (MIT)/Graybiel Lab UROP/DeepLabCut Project/Shank3AutismMouseModel-qsimeon-2020-07-06/config.yaml'
>>> import os
>>> Homecage_directory = '/Users/qsimeon/Dropbox (MIT)/SCSB Mouse 2018 SFARI/DeepLabCut/2019-08-19 Shank WT KO pilot/Day 3 (8-21-19)'
>>> for vid in os.listdir(Homecage_directory):
...     if vid.startswith('.'):
...             continue
...     deeplabcut.add_new_videos(config_path, videos=[os.path.join(Homecage_directory, vid)], copy_videos=False)
>>>
>>> Truscan_directory = '/Users/qsimeon/Dropbox (MIT)/SCSB Mouse 2018 SFARI/DeepLabCut/2019-12 Shank DREADD Truscan Videos/D3_2019-12-11/group2'
>>> for vid in os.listdir(Truscan_directory):
...     if vid.startswith('.'):
...             continue
...     deeplabcut.add_new_videos(config_path, videos=[os.path.join(Truscan_directory, vid)], copy_videos=False)
... 
Creating the symbolic link of the video
New video was added to the project! Use the function 'extract_frames' to select frames for labeling.
……………

Edit the following parameters in the config.yaml file inside the project folder after project creation. The first lines below are choosing the crop coordinates for each test video (I used the same crop coords for all 4; but I recommend choosing individual coordinates for each video such that the outline of the behavior box is obtained.):

Below the entire config.yaml file is pasted:
    # Project definitions (do not edit)
Task: Shank3AutismMouseModel
scorer: qsimeon
date: Jul6
multianimalproject: false

    # Project path (change when moving around)
project_path: /Users/qsimeon/Dropbox (MIT)/Graybiel Lab UROP/DeepLabCut Project/Shank3AutismMouseModel-qsimeon-2020-07-06

    # Annotation data set configuration (and individual video cropping parameters)
video_sets:
  ? /Users/qsimeon/Dropbox (MIT)/SCSB Mouse 2018 SFARI/DeepLabCut/2019-12 Shank DREADD
    Truscan Videos/D3_2019-12-11/group2/5181_60minHabD3.00.avi
  : crop: 1, 693, 5, 599
  ? /Users/qsimeon/Dropbox (MIT)/SCSB Mouse 2018 SFARI/DeepLabCut/2019-08-19 Shank
    WT KO pilot/Day 3 (8-21-19)/Ms4791_Day3.00.avi
  : crop: 18, 795, 56, 587
  ? /Users/qsimeon/Dropbox (MIT)/SCSB Mouse 2018 SFARI/DeepLabCut/2019-08-19 Shank
    WT KO pilot/Day 3 (8-21-19)/Ms4800_Day3.00.avi
  : crop: 55, 758, 60, 519
  ? /Users/qsimeon/Dropbox (MIT)/SCSB Mouse 2018 SFARI/DeepLabCut/2019-08-19 Shank
    WT KO pilot/Day 3 (8-21-19)/Ms4804_Day3.00.avi
  : crop: 24, 798, 55, 591
  ? /Users/qsimeon/Dropbox (MIT)/SCSB Mouse 2018 SFARI/DeepLabCut/2019-08-19 Shank
    WT KO pilot/Day 3 (8-21-19)/Ms4805_Day3.00.avi
  : crop: 18, 798, 56, 582
  ? /Users/qsimeon/Dropbox (MIT)/SCSB Mouse 2018 SFARI/DeepLabCut/2019-08-19 Shank
    WT KO pilot/Day 3 (8-21-19)/Ms4806_Day3.00.avi
  : crop: 3, 769, 53, 573
  ? /Users/qsimeon/Dropbox (MIT)/SCSB Mouse 2018 SFARI/DeepLabCut/2019-08-19 Shank
    WT KO pilot/Day 3 (8-21-19)/Ms4807_Day3.00.avi
  : crop: 56, 753, 45, 506
  ? /Users/qsimeon/Dropbox (MIT)/SCSB Mouse 2018 SFARI/DeepLabCut/2019-08-19 Shank
    WT KO pilot/Day 3 (8-21-19)/Ms4819_Day3.00.avi
  : crop: 32, 797, 76, 592
  ? /Users/qsimeon/Dropbox (MIT)/SCSB Mouse 2018 SFARI/DeepLabCut/2019-08-19 Shank
    WT KO pilot/Day 3 (8-21-19)/Ms5111_Day3.00.avi
  : crop: 9, 769, 21, 532
bodyparts:
- snout
- tailbase
- leftear
- rightear
- leftflank
- rightflank
- leftfrontpaw
- rightfrontpaw
- leftbackpaw
- rightbackpaw
start: 0.4
stop: 0.8
numframes2pick: 20

    # Plotting configuration
skeleton:
- - snout
  - tailbase
- - snout
  - leftear
- - snout
  - rightear
- - snout
  - leftflank
.
. 
. <N-choose-2 pairs, where N is the number of labelled body parts>
skeleton_color: cyan
pcutoff: 0.8
dotsize: 8
alphavalue: 0.7
colormap: plasma

    # Training,Evaluation and Analysis configuration
TrainingFraction:
- 0.85
iteration: 0
default_net_type: mobilenet_v2_1.0
default_augmenter: default
snapshotindex: -1
batch_size: 8

    # Cropping Parameters (for analysis and outlier frame detection)
cropping: false
croppedtraining: false
    #if cropping is true for analysis, then set the values here:
x1: 0
x2: 640
y1: 277
y2: 624

    # Refinement configuration (parameters from annotation dataset configuration also relevant in this stage)
corner2move2:
- 50
- 50
move2corner: true


Data selection (extracting frames) using the GUI:


This is equivalent to the Terminal command (continuing from the last ones):
`deeplabcut.extract_frames(config_path, mode='automatic', algo='uniform', userfeedback=True, crop=GUI)`

Random selection (algo=‘uniform’) of frames works best for behaviors where the postures vary across the whole video (this is our scenario). We will extract 20 - 30 frames per video (so that’s a max of 120 frames with just 4 videos; DeepLabCut authors suggest labelling ~50-200 frames for robust generalization).
 
Label frames now: use the GUI (recommended) or run the Terminal command (continuing in the window from the previous commands; this will launch the GUI):
deeplabcut.label_frames(config_path)

IDEA: Collaborative Labelling
The latest project folder is called Shank3AutismMouseModel-qsimeon-2020-07-06 and it is …...


<Screenshot of GUI labelling of frames>

Then visualize labels by running the Terminal command:
`deeplabcut.check_labels(config_path)`
Note that by default the human labels are plotted as plus (‘+’), DeepLabCut’s predictions either as ‘.’ (for confident predictions with likelihood > p-cutoff) and ’x’ for (likelihood <= p-cutoff).


Train and test network on Google Colab (too slow on a PC!)
Before training the network, migrate the whole working directory (‘Shank3AutismMouse-author-dateofcreation’) to Google Drive or GitHub. 

After migrating to google Drive, change the project_path and resnet parameters in config.yaml:
project_path: /content/drive/My Drive/Shanks3AutismMouse-qsimeon-2020-04-20
resnet: 50

I have a Google Colab notebook for training and testing the network: drive.google.com/open?id=10gq-j5FElV127jhE_jpBz7CrUutoww5Z

	
Steps to do in a Google Colab notebook
Create Training Datasets and Train Network(s):

 
Modify parameters in the train pose_cfg.yaml file of all model shuffles:
init_weights (for resnet_50): /content/drive/My Drive/Shank3AutismMouse-qsimeon-2020-04-20/dlc-models/<path-to-subdirectory-of-exiting-pretrained-weights-of-existing-resnet50-shuffle/snapshot-<LastSnpashotNumber>
OR
init_weights (mobilenet_v2_1.0): /content/drive/My Drive/Shank3AutismMouse-qsimeon-2020-04-20/dlc-models/<path-to-subdirectory-of-exiting-pretrained-weights-of-existing-mobilenet-shuffle/snapshot-<LastSnpashotNumber>

multi_step:
- - 0.005
  - 200000
- - 0.008
  - 430000
- - 0.002
  - 730000
- - 0.001
  - 1030000

pos_dist_thresh: 15
 
 
Evaluate the Network:

 
 
