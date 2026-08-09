---
layout: post
title: "Glove design for hand motion capture"
date: 2026-08-09
---

- working on an idea for the glove design for capturing human hand data. 
- Goal: low cost, no fluff, miminal dexterous hand capturing glove
- the exoskeleton or framework for hand motion capture

Studies examples of the hand capture gloves show current are cumbersome, too bulky and cannot be used in a factory of 500/1000 people to capture data on a large scale. One off from the lab is great. 
So i went ahead and simplified the design or found an inspiration for it. 
A couple of concerns when starting out. First the knuckles present 2 degrees of freedom. That needs to be accomodated. 
Secondly, i want it to an active motion capture glove not a passive one. A bunch of chinese companies are working towards passive capture nowadays. I think they are both costly, not well suited for long term usage and too bulky. 
The closest I have come to is the mimic glove. I do suspect they are using the same mechanism as I have found for the knuckle problem solving and they are the closest to an operational form factor. 

When it comes to the design of the hand, Art glove is a real inspiration. The design is closest to what i would want in the next glove. However, i think using a combination of 3D printing and sheet metal will certainly reduce the bulkly part. 

At this stage a lot of the gloves provide hand motion capture  with little or somewhat experimental tactile sensors added. 
IMO, the tactile part is to integrated as a glove/knitwear over this hard exoskeleton- perhaps 2 seperate modules combined. 


The tactile glove sensor perhaps come in 3 different solutions
1. Normal force only- using the piezoresitive touch sheet. The code and design are open sourced
2. Bought from chinese supplier- fairly new and the sdk from a few designs give this way. Cheapest, fastest and no development work to get the sensors that can measure 3D forces and shear as well
3. A combination of the two. For the palm and other broader sensors use the sheet. for the tips, use the as bought or purchased part


### Mimic hands
![Mimic_glove](/assets/posts/dexterous-joint-collection-glove/mimic_snapshot.png)


![mimic2](/assets/posts/dexterous-joint-collection-glove/mimic_hammer.webp)

- in the palm area one can see the underparts have a circular and round section for the knuckles degree of motion
![alt text](/assets/posts/dexterous-joint-collection-glove/mimic_dofs.webp)

What i recommend the design to look like for reduction of bulk weight- referenced from the naked prosthetics gloves
- All 4 fingers
![Naked_prosthetics](/assets/posts/dexterous-joint-collection-glove/naked_prosthetics_fingers.png)


- thumb design 

![alt text](/assets/posts/dexterous-joint-collection-glove/thumb2.png)
![alt text](/assets/posts/dexterous-joint-collection-glove/thumb1.png)

1. the mechanism - 4 bar linkage can be dropped. That is supposed to drive the hand through. for our purpose we dont need that.
![fingers](/assets/posts/dexterous-joint-collection-glove/art_glove_fingers.png)

2. for the finger insertion pads, we can use this- art glove for the inspiration. 


For the sensors, using the prebuilt PCB for the magnet based postion sensing is the best. It also much smaller in dimension than buying an off the shelf part. 

TAG provides the reference electronics as well. 
"Feel Robot Feels: Tactile Feedback Array Glove for Dexterous Manipulation". 

# Tactile
Use of the shelf paxini-gen3
![Paxini](/assets/posts/dexterous-joint-collection-glove/paxini.png)

an alternative is the agiletact sensor

for the entire hand spread, i recommend the viral glove heatmap from x. 
![x](/assets/posts/dexterous-joint-collection-glove/glove-heatmap.jpg)

the underlying piece of technology behind this from fpcb-glove design

---

**Links:** [GitHub repo](https://github.com/skr3178/Dexterous-joint-collection-glove) (writeup, reports, my CAD sources) · [HuggingFace dataset](https://huggingface.co/datasets/sangramrout/Dexterous-joint-collection-glove) (full ~6.5 GB research archive: reference glove/hand repos, CAD, papers, videos)
