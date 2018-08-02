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

Step 1: Downlaod the repo and “cd” to the training_scripts folder via terminal
Step 2: Create a folder and place all your images inside
Step 3: Create another folder with a single text file containing the annotations.

The format for the annotations text file is this:

"{filename}.json#0 {caption}"

Here is an example caption:
PIA11242.jpg.json#0 mudstone and dark sand dune field

This should be a single text file of all your captions that will be preprocessed by the 
"preprocess_data.sh" script below

Step 4: Run "sudo chmod a+x *" to make all files executable

Step 5: Run "sudo ./init.sh" which takes two arguments in the followng order: the path to your workspace directory 
and the path to the directory where you want you want the processed captions to go 

                ex. ./init.sh /Users/bhavin/img2text /Users/bhavin/img2text/MARS_processed_captions


Step 6: Run "sudo ./preprocess_data.sh" which takes 4 arguments in the following order: the path to your workspace directory, the path to your image folder, the path to your unprocessed caption file, and the processed captions dir specified in step 5

                ```
                ex. ./preprocess_data.sh /Users/bhavin/img2text /Users/bhavin/img2text/MARS_imgs 
                /Users/bhavin/img2text/MARS_text/mars.token.txt /Users/bhavin/img2text/MARS_processed_captions 
                ```

You should have generated an index.json file in the directory you specified (ex. MARS_processed captions). It will also generate a word_counts.txt file in the TFRecords directory.

Step 5: Run "sudo ./start_train.sh" which takes three arguments in the followng order: the path to your workspace directory, whether or not to train
                        the inception frontend, and the number of training steps

                ```
                ex. "sudo ./start_train.sh /Users/bhavin/img2text/ false 8360"
                ```

Step 6: Begin training! Your model file will be generated in the Model directory

Questions, comments?
===================
Send them to [Chris A. Mattmann](mailto:chris.a.mattmann@jpl.nasa.gov) or [Thejan Wijesinghe](mailto:thejan.k.wijesinghe@gmail.com).

Contributors
============
* Chris A. Mattmann, JPL & USC
* Thejan Wijesinghe, University of Moratuwa

License
===
This project is licensed under the [Apache License, version 2.0](http://www.apache.org/licenses/LICENSE-2.0).






