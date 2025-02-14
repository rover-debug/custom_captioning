# img2text
This project contains a [Tensorflow](http://tensorflow.org/) trained model that implements the [GSOC 2017 project Tensorflow Image to Text in Apache Tika](https://wiki.apache.org/tika/GSOC/GSoC2017) based on the paper [Show and Tell: A Neural Image Caption Generator](https://arxiv.org/abs/1411.4555). The model is split into multiple parts, and the associated [Docker image](https://raw.githubusercontent.com/apache/tika/master/tika-parsers/src/main/resources/org/apache/tika/parser/captioning/tf/Im2txtRestDockerfile) in [Apache Tika](http://tika.apache.org/) puts the model back together during the docker process for use in Tika.

How to obtain the checkpoint file?
===================
```
git clone https://github.com/USCDataScience/img2text.git
cd img2text
sudo chmod a+x download-1M_iters_ckpt.sh
./download-1M_iters_ckpt.sh
```

How to retrain with your data?
=======================

Step 1: Downlaod the repo and “cd” to the training_scripts folder via terminal. This training_scripts folder is your "Workspace directory" which will be referenced below.

Step 2: Create a folder and place all your images inside

Step 3: Create another folder with a single text file containing the annotations.

The format for the annotations text file is this:

"{filename}.json#0 {caption}"
"{filename}.json#0 {caption}"
"{filename}.json#0 {caption}"
"{filename}.json#0 {caption}"

Here is an example caption:

PIA11242.jpg.json#0 mudstone and dark sand dune field

Every image needs a line with its filename and caption.

Step 4: Run "sudo chmod a+x *" to make all files executable

Step 5: Run "./init.sh" which takes two arguments in the followng order: the path to your workspace directory 
and the path to the directory where you want you want your processed captions to go (this folder can be named anything)

                	./init.sh /Users/bhavin/img2text/training_scripts /Users/bhavin/img2text/MARS_processed_captions

Step 6: Run "./preprocess_data.sh" which takes 4 arguments in the following order: the path to your workspace directory, the path to your image folder, the path to your unprocessed caption file, and the processed captions dir specified in step 5

                
                ./preprocess_data.sh /Users/bhavin/img2text/training_scripts /Users/bhavin/img2text/MARS_imgs 
                /Users/bhavin/img2text/MARS_text/mars.token.txt /Users/bhavin/img2text/MARS_processed_captions 
                

This script will generate an index.json file in the directory you specified (ex. MARS_processed captions). It will also generate a word_counts.txt file in the TFRecords directory. These will feed into our training operation.

Step 5: Run "./start_train.sh" which takes three arguments in the followng order: the path to your workspace directory, whether or not to train the inception frontend, and the number of training steps

                
                sudo ./start_train.sh /Users/bhavin/img2text/training_scripts false 8360
                

Step 6: Begin training! Your model file will be generated in the Model directory as a .meta file and a data file

model.ckpt-8360.meta - This is a metagraph file which contains the Tensorflow computational graph
model.ckpt-8360.data-00000-of-00001 - This is a file containing all the weights for each tensor in the metagraph

Step 7: To use these two files, you can create a simple Python script that imports the tensorflow library and loads up these files into a model. You can then feed the model with any images you'd like and watch it classify them. https://www.tensorflow.org/api_guides/python/meta_graph#Import_a_MetaGraph

Alternatively: You can also use Apache Tika to run this model on a web server with a simple-to-use API. Use these instructions to start up a docker container with Tika and its pretrained captioning model: https://wiki.apache.org/tika/ImageCaption. To use your newly trained model, simply ssh into the container and replace the existing checkpoint files with your model files.


Questions, comments?
===================
Send them to [Chris A. Mattmann](mailto:chris.a.mattmann@jpl.nasa.gov) or [Thejan Wijesinghe](mailto:thejan.k.wijesinghe@gmail.com) or [Bhavin Shah](mailto:bhavints@usc.edu)

Contributors
============
* Chris A. Mattmann, JPL & USC
* Bhavin Shah, JPL & USC
* Thejan Wijesinghe, University of Moratuwa

License
===
This project is licensed under the [Apache License, version 2.0](http://www.apache.org/licenses/LICENSE-2.0).






