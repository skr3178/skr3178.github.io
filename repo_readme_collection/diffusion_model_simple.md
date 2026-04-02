# Diffusion Deep Learning AI Project

## Training Dataset
Based on the code, the dataset used for training is a custom sprite dataset consisting of:
- `sprites_1788_16x16.npy` - Contains 1,788 sprite images, each 16×16 pixels
- `sprite_labels_nc_1788_16x16.npy` - Contains corresponding labels for the sprites

## Architecture and Training

### UNet Architecture

In the architecture, the model uses a downsampling and then further upsampling. 
First, the downsampling is done with the use of convolution layers by adding some minuscle levels of noise. 

Thereafter, upsampling is done with the aid of contexts (multiplied) and time (added).
![UNet Architecture](UNet.png)


### Training and Loss Computation
![Training and Loss Computation](Training%20and%20loss%20computation.png)

The training process consists of adding a small amount to noise, and then asking to model (NN) to guess what that noise added is. The MSE loss (actual noise, predicted noise) helps train the model.

The distribution of the noise is non uniform, with different techniques better suited for different levels of training. 

DDPM - better suited for higher training steps - Uses Markov process (probablistic model) and hence slower. Markov process is better suited for application where one step happens after the other.  

DDIM - Better suited for lower training steps - forgeoes Markov process and hence faster. 

Convolution is added

### Adding Context to the Model
![Add Context](Add_context.png)

Adding context to the model is through the use of contextual text information. 
