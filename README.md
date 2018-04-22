## Face Generation TensorFlow


* Keras-gpu   
* TensorFlow  
* CUDA


## Conda configuration
##
source activate face_GAN

## Post activated packages installed
##
conda install --name face_GAN tqdm
pip install Image


# Configuring Amazon EC2 instance

This model requires GPU architecture to learn. Using CPU by itself would require 1 week to run.

I elected to use Amazon EC2 because the GPU setup was simple. My local GPU is ok but the setup to get it to work was not trivial.

It is possible using anaconda by itself (conda install -c anaconda keras_gpu) MAY be simple to use. 
However my experience using docker locally never satisfied the requirements to use GPU.
