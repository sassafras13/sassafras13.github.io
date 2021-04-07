---
title: AFM Scanning Parameters and Image Artifacts
layout: post
---

I have been writing a series of posts on different aspects of atomic force microscopy, starting with this [post](https://sassafras13.github.io/AFM/) describing the fundamental operating principles, and this [post](https://sassafras13.github.io/ProbeSelection/) on how to choose a probe for your application. Right now, I want to talk about two related aspects of AFM imaging: how to optimize your scanning parameters, and what common image artifacts you should look out for while you’re imaging. Let’s get to it!

## Optimizing Scanning Parameters

I will start by introducing some of the parameters we commonly tune during AFM imaging, and then I will describe each in more detail below. This information is also summarized in Figure 1. The first parameter we typically control is the **setpoint**. This parameter defines how much interaction force the system should maintain between the probe tip and the sample surface. Next, we have the **drive amplitude**, which is the amplitude at which the cantilever and tip are oscillating in tapping mode. (I don’t believe I have access to this parameter in my setup, so I will only briefly touch on it here.) We also have control over the control system’s **gains**, which determines the sensitivity of the control loop. We can change the **scan rate** or **scanning speed** while we are imaging, which means we change how quickly the probe rasters over the sample surface. Finally, we can choose the **resolution** of our scan image to determine how many pixels we want in our image [1]. 

![Fig 1]({{ site.baseurl }}/images/2021-04-07-ImageArtifactsScanParams-fig1.png "Figure 1"){:width=75%}     
Figure 1 - Source: [1]     

### Setpoint

The setpoint tells the AFM control loop what value of force interaction the probe should maintain with the sample while scanning. I think the actual value that the setpoint is controlling is either the distance between the probe and the sample or the vibration amplitude that the system is maintaining [1]. If you choose a larger setpoint value, you are asking the system to hold the probe farther away from the sample surface, and therefore you want a lower amount of interaction force between the probe and the sample. **If your image is too noisy, you should increase your setpoint.** Conversely, bringing the setpoint down increases the interaction between the probe and the sample. **Decrease the setpoint for better quality images** [1].

### Driven Amplitude

The driven amplitude is the amplitude of the oscillations applied by the AFM setup during tapping mode scanning. If we increase the drive amplitude, we can improve our phase signal data, to some extent [1]. 

### Gain

The gain amplifies the signal from the probe. Increasing the gain can help improve signal quality, but it will also amplify signal noise [1]. 

### Scan Rate/Speed

This determines how quickly the probe travels over the sample surface. Generally speaking, a slower scan speed will lead to better images, but the tradeoff is that each image will take more time to collect [1]. 

### Resolution 

A high resolution will lead to better image quality, but it will also greatly increase the amount of time it will take to collect that image [1]. 

Before we move on to discuss image artifacts, let’s talk about what order you should tune your parameters in:

* Generally speaking, the setpoint and the gain should be the two parameters you change first as you’re trying to optimize your images [1]. Some advice I was given while training on the AFM was that you should always increase your gain first, and decrease your setpoint second.   

* If your signal is very noisy, try decreasing the gain. If that does not work, try modifying the scan rate - it could be too fast _or_ too slow so try playing around with it to see if it improves your image quality [1].    

* Check the trace and retrace curves as you are scanning. This [poster](https://www.nanoandmore.com/pdfs/how-to-optimize-AFM-scan-parameters.pdf) is a great reference for what your trace and retrace curves for imaging speed, gain and setpoint should look like [2]. 

## Image Artifacts

Now that we have some idea of what our scanning parameters are and how we can tune them, let’s talk about some of the image artifacts we commonly see and what clues they give us about how we can improve our scanning parameters. Typically, image artifacts can be broken down into the following categories: **probe** artifacts, **scanner** artifacts, **image processing** artifacts,  **noise** and **process** artifacts [3]. Note that an artifact is any feature in an image that shouldn’t be there (i.e. that does not really exist on the sample surface) [3]. 

### Probe Artifacts
 
The probe tip is the part of the AFM setup that is directly interacting with the sample surface, so if it is damaged in any way, this can lead to image artifacts. Chipped probes can make everything look like it's the same size, and triangular, because the tip is being smashed into the sample surface or is being dragged along the surface too quickly [3]. The tip can also pick up dirt or particulate from the sample, which can add to the chip geometry, or create a secondary tip, which can make the image look like you are seeing double features (i.e. as though you had crossed your eyes while looking at the image) [3]. 

### Scanner Artifacts

The piezoelectric stage is responsible for moving the sample relative to the probe, so it can also contribute artifacts if it is not operating properly. The stage will always suffer from some hysteresis and creep, which can add artifacts to an image. You should be able to identify this by scanning a calibration sample and measuring the dimensions of a reference feature and determining how far off your measurements are [3]. Hysteresis especially can create “odd” images. Also, be especially careful of hysteresis if you are scanning in an extreme region in x, y or z (i.e. near the maximum or minimum of their ranges). You are more likely to see errors due to poor calibration or hysteresis in these regions far from the center of the operating envelope [3]. If the background appears curved, this can also be caused by hysteresis of the stage, or if the sample was not set up on a level surface [3]. 

### Image Processing Artifacts

There are a couple of things that you can do in the post-processing step that can negatively impact your images. For example, if you do line leveling, this can create bands in the image. You should use a mask to exclude features to remove these bands while doing line leveling. Filtering can also change the feature dimensions (especially) in z, so be careful of reporting incorrect dimensions if you have used filtering. Filtering (such as Fourier filters) can also add nodules that contain a lot of noise to an image [3]. 

It is good practice to collect z-height measurements from a histogram of multiple measurements instead of taking the measurement from a single line profile. Also, be wary of images that have no noise at all - this is more likely to indicate that something is wrong than that you are a genius AFM tuner [3]. 

### Sources of Noise

Some common sources of noise in your image can come from ambient vibrations (construction, vehicles outside, etc.) or sample contamination. Electronic noise from the machine setup can also interfere with your images - try moving the scan or driven vibration frequencies to eliminate this noise. 

### Process Artifacts

If you are scanning too quickly, then you may not give the probe enough time to smoothly travel over sample surfaces and accurately measure them. If the features look odd or misshapen, try slowing down the scan speed. If the features that you are measuring are too large, this is also a good indication that you need to alter the setpoint and scan speed. You can also try scanning in different directions - if a particular feature exists in images taken while scanning in two different directions, it is more likely to be real than if it only shows up when scanning in a particular direction. ANd finally, if you see low frequency waves in the image, this can be caused by light reflecting off the sample surface, which means that you have not properly centered the laser on the probe cantilever [3]. 

Alright, I think that is about all I have to say about AFM imaging for today. Thanks for reading!

## References:

[1] “Step-by-step instruction for using the Digital Instruments Multimode AFM.” <http://mcf.tamu.edu/wp-content/uploads/2016/09/MultiMode-AFM-Instructions_Sep2010.pdf> Visited 07 Apr 2021. 

[2] “Choose your AFM settings properly!” NanoAndMore. <https://www.nanoandmore.com/pdfs/how-to-optimize-AFM-scan-parameters.pdf> Visited 07 Apr 2021. 

[3] “Artifacts in AFM Images.” AFM Workshop seminar. <https://www.afmworkshop.com/learning-center/afm-webinars> Visited 07 Apr 2021. 
