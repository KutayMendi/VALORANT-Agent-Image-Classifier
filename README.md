# VALORANT-Agent-Image-Classifier
This project is me messing around with training a simple image classifier on Valorant agents. I split it into two notebooks: the image processing first, then the ML model.

Dataset
-------
Creating a data set was not my main goal for this project, but I couldnt find one online so I had to. The process was quite time consuming and tedious so instead of using a variety of maps and all agents, I used a subset.
I recorded the dataset myself by loading into Haven in observer mode and asking two friends to cycle through 15 different agents (Credit to my good friends Owen and William). For each agent I recorded 15s clips (1080p, 30fps) of them doing 8 actions:
-Idle
-Walking
-Running
-Crouching
-Jumping
-Turning 360
-Ability
-Weapons
While recording, I rotated the camera so each clip had multiple angles. Afterwards, I cut the framerate way down to 3 fps. The reason is just practical higher fps gave me way too many nearly identical frames and my GPU wasn’t happy about it. At 3 fps I still keep variety without flooding the dataset with too much waste.

Processing
---------
-Extracted frames from the clips.
-Added some basic thresholds/blur checks to drop junk frames. For example, I tweaked blur threshold values a couple of times (120→100) so motion blur wouldn’t pollute the dataset.
-Stored images per agent folder.

Training
-------
-Used ResNet18 (pretrained) with a simple Adam optimiser, rather than building a new CNN.
-Trained for 5 epochs (any more wasn’t giving big improvements on my hardware).
-Logged training/validation loss and accuracy each epoch.
-Ended up with ~97% test accuracy overall, though some agents (like Fade and Chamber) were trickier.

While building the model:
--------
-The blur filtering was too strict and dataset shrank too much, so I adjusted the parameter
-FPS higher than 3 led to slowdown and repetitive data. FPS lower than 3 meant not enough samples, especially for training a neural net
- I Stuck with a middle ground that worked fine on my 3060 Ti, which is quite a powerful card even in 2025

What I learnt:
-------
My main aim for this project was to practise my python for machine learning skills, so I aimed to incorporate as many different packages as possible (eg pandas, numpy, matplotlib, pytorch, etc). In this regard, although I often needed help (googling, using online tutorial etc), I learnt a lot from this project
However, one of the biggest takeaways I got from this project is that a ML model is only as good as the data you train it on. If I have time, I would like to create a better data set with more variation, especially because the bias introduced into the model due to the dataset is quite apparent. For example, agents like breach didnt have enough images of him holding his weapon (mostly had his knife out). Thus, when presented an image of breach holding a vandal (weapon), the model would get confused and not get it correct.
