# Face Generation TensorFlow

In this project I use generative adversarial networks to generate new images of faces.



# Software  

# Amazon EC2  
* Anaconda  
* CUDA  
* Jupyter notebooks  
* Keras-gpu    
* Python 3.6   
* TensorFlow  


## Conda configuration
##
source activate face_GAN

## Post activated packages installed
##
conda install --name face_GAN tqdm
pip install Image


## Configuring Amazon EC2 instance

This model requires GPU architecture to fit the model (train). Using CPU by itself would require 1 week to run.

I elected to use p2.xlarge AWS EC2 because the GPU setup was simple. My local GPU is ok but the setup to get it to work was not trivial. Nightmares linger from the black screen after updating Nvidia drivers on Ubuntu. Also cuda and libcudnn install and configuration is required. 

It is possible using anaconda by itself (conda install -c anaconda keras_gpu) MAY be simple to use. 
However my experience using docker locally never satisfied the requirements to use GPU.

A [Good Setup Tutorial](https://chrisalbon.com/software_engineering/cloud_computing/run_project_jupyter_on_amazon_ec2/) is available showing how to configure TensorFlow-GPU and Jupyter Server.   

First I setup a EC2 instance on AWS using GPU computation. The community box "Deep Learning AMI Ubuntu Version 7.0" comes with latest binaries of deep learning frameworks pre-installed in separate virtual environments: MXNet, TensorFlow, Caffe, Caffe2, PyTorch, Keras, Chainer, Theano and CNTK. Fully-configured with NVidia CUDA, cuDNN and NCCL as well as Intel MKL-DNN. The box has 75 GB of memory. It is easy to ssh in and jump into Jupyter using two commands:  

source activate tensorflow_p36
jupyter notebook

Before using Jupyter some configuration is required. For Jupyter to be up and running:    

`ssh -i "tf_keras_gpu.pem" ubuntu@ec2-18-219-93-211.us-east-2.compute.amazonaws.com`  

jupyter notebook --generate-config  
`## Writing default config to: /home/ubuntu/.jupyter/jupyter_notebook_config.py`   

Next, open python to generate password (this is the password you will use when you remotely log into your Jupyter Notebook - so remember it):   

`python3`  
`Python 3.5.2 (default, Nov 17 2016, 17:05:23)`  
`[GCC 5.4.0 20160609] on linux`   
`Type "help", "copyright", "credits" or "license" for more information.`   
`>>> from notebook.auth import passwd`    
`>>> passwd() `   
`Enter password:  `  
`Verify password:  `  
`'sha1:608e15353a43:f2054d495b7992ec518ada4cd76e844d738e58bd'`  
`quit()`   


In n a new terminal session open ` 	nano ~/.jupyter/jupyter_notebook_config.py` . Add this at the beginning of the document:

`c = get_config()`  

`# Kernel config`   
`c.IPKernelApp.pylab = 'inline' `   # if you want plotting support always in your notebook

Notebook config  
`c.NotebookApp.certfile = u'/home/ubuntu/certs/mycert.pem'`   # location of your certificate file
`c.NotebookApp.ip = '*'`  

` # c.NotebookApp.open_browser = False `  #so that the ipython notebook does not opens up a browser by default  
`c.NotebookApp.password = u'sha1:xxx...xxx' ` #the encrypted password we generated above

Set the port to 8888, the port we set up in the AWS EC2 set-up  
`c.NotebookApp.port = 8888`   

Save and Exit config file

Create folder for notebooks

cd ~

mkdir Notebooks

cd Notebooks

### Start Jupyter Notebook Server   
source activate tensorflow_p36
jupyter notebook

https://ec2-18-219-93-211.us-east-2.compute.amazonaws.com:8888

## Version Control: Keeping Track of Code Changes

All document changes are saved for version control, using GitHub. The model has many iterations and all optimizations will be recorded. 


`echo "# face_generation_GAN" >> README.md`    
`git init`    
`touch .gitignore`   
`git add *`     
`git commit -m "first commit"`  

**Only need `remote add` for the first time connecting to git**   
`git remote add origin https://github.com/prfrl/face_generation_GAN.git `

`git push -u origin master`      

**Refresh .gitignore cache**  Do not forget period at end of cached.     
`git rm -r --cached .`   
