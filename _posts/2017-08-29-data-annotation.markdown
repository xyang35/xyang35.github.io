---
layout: post
comments: false
title: "Data Annotation Instruction"
excerpt: "Data annotation instruction for single image haze removal project."
date: 2017-08-29
mathjax: false
---
 <div style="background-color:rgba(255, 255, 0, 0.7); text-align:center; vertical-align: middle;">
[***Sign-up sheet is here***](https://docs.google.com/spreadsheets/d/15tR6jz6aC5bigkq8bbTq4U8xExHKX9SokeFBrL004hE/edit?usp=sharing)
</div>

## Introduction

First of all, thank you very much for your help! Your contribution will be gratefully acknowledged in our new paper. :)

Our project is about “single image haze removal”, meaning that we would like to remove the haze from the hazy input and recover the haze-free image. **Your work is to annotate an image is hazy or not**. You can access the annotation webpage using the address provided in the [***sign-up sheet***](https://docs.google.com/spreadsheets/d/15tR6jz6aC5bigkq8bbTq4U8xExHKX9SokeFBrL004hE/edit?usp=sharing). Each session contains 10 pages, while each page contains 50 images. It takes **only 10 minutes** to finish a session! 

***Before*** you start working on a session, please open the [***sign-up sheet***](https://docs.google.com/spreadsheets/d/15tR6jz6aC5bigkq8bbTq4U8xExHKX9SokeFBrL004hE/edit?usp=sharing) and put your intitial in the box of the group and session you want to work on. For each image, we need three different people to annotation it, so please choose only ***one of the group*** for each session. 

Each page contains 50 images, and you need to submit your annotation after you finish all of them. The annotation will be stored **only after** you click the “Submit” button. The “Submit” button is at the bottom of the webpage. The default selection is “haze-free”, so you only need to select the other options if you find the image should not be haze-free. After clicking the submission button, you will be redirect to the index page, then you can choose the next session.

Some kind reminder:

- Put your initial in the [***sign-up sheet***](https://docs.google.com/spreadsheets/d/15tR6jz6aC5bigkq8bbTq4U8xExHKX9SokeFBrL004hE/edit?usp=sharing) **before** working on your session

- Different groups have different index pages, don't mess up
- Click "Submit" button after finishing a page to store your annotation
- Read the examples below first to get the annotation criteria
- Don’t waste too much time on struggling the correct annotation, leave the ambiguous ones “not sure”.

## Annotation Criteria

In our project, “haze” can be viewed as a general term including haze, fog and smoke. In Chinese we can call it “雾” or “霾”. The existence of haze always degrades the image and can be visually recognized by humans. Some examples of hazy images and the corresponding dehazing results are showed as follows.

<p align="center" >
    <img src="/assets/annotation/example1.png" align="center" height="280px" alt>
</p>

In our dataset, we need to manually annotate each image as one of the following four labels:
- **Hazy**: the image should satisfy both two criteria: 1. It is hazy; 2. **It is good for haze removal task** (we can expect decent visual improvement if the haze is removed)
- **Not sure**: may contain a little haze, but just a small part of the image (even though we remove the haze, the quality does not differ much)- **Haze-free**: no haze, clean image.- **Bad image**: the option in case you see some images that should not be included in our dataset. For example, images **at night, with low resolution or extreme environment** (too bright or too dark or anything weird).## Examples

### Hazy

<p align="center" >
    <img src="/assets/annotation/example2.png" align="center" height="280px"alt>
    <em>Definitely hazy image, and we can expect great improvement after haze removal. So annotate it as <b>Hazy</b>.</em>
</p>

<p align="center" >
    <img src="/assets/annotation/example3.png" align="center" height="280px"alt>
    <em>The haze is not as strong as the one above, but it is still visible and makes the buildings behind blurry. You can annotate it as <b>Hazy</b> in these cases.</em>
</p>

<p align="center" >
    <img src="/assets/annotation/example45.png" align="center" height="280px"alt>
    <em>The left image is <b>hazy</b>. It is sometimes useful to justify an image according to its neighbors. For example, the neighbors of this image is on the right. We can then easily tell which ones are hazy and which ones are not.  </em>
</p>

### Not sure

<p align="center" >
    <img src="/assets/annotation/example6.png" align="center" height="280px"alt>
    <em>Haze exists at the place very far away. Even though we know there’s haze there, we expect that it may not have much different after haze removing, so we can annotate it as <b>not sure</b>.</em>
</p>

<p align="center" >
    <img src="/assets/annotation/example7.png" align="center" height="280px"alt>
    <em>You may see some haze at the background, or may not. In these cases, you can annotate them as <b>not sure</b>.</em>
</p>

### Haze-free

<p align="center" >
    <img src="/assets/annotation/example8.png" align="center" height="280px"alt>
    <em>Haze-free images are <b>haze-free</b>.</em>
</p>

### Bad image

<p align="center" >
    <img src="/assets/annotation/example9.png" align="center" height="280px"alt>
    <em>We don’t need this kind of images with extreme dark and bright. So annotate it as <b>Bad image</b>.</em>
</p>

<p align="center" >
    <img src="/assets/annotation/example10.png" align="center" height="280px"alt>
    <em>We don’t need the images at night in our experiment, so annotate it as <b>Bad image</b>.</em>
</p>

## Closure

Again, we sincerely appreciate your help to our project. Hope that you will enjoy watching different moments of the scenes in Beijing. Please let me know if you have any question or meet any problem. ***My email address is: yangxitongbob@gmail.com***.P.S. This dataset is collected by Yuncheng Li at VIStA Lab and it is not public yet. Please don’t share the data to other people.