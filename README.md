# Number Plate Detection using Attention OCR

## Problem

I have used tensorflow attention-ocr to predict the text present on number plates. The reference tutorial article which I followed can be found [here](https://nanonets.com/blog/attention-ocr-for-text-recogntion/)

Refer [this](https://github.com/tensorflow/models/tree/master/research/attention_ocr) for tensorflow research model

## Usage

Make sure you have installed tensorflow version 1.15, and latest version of Open CV and Pandas

```
python3 -m venv ~/.tensorflow
source ~/.tensorflow/bin/activate
pip install --upgrade pip
pip install --upgrade tensorflow-gpu=1.15
```

## Getting training data
We have [images of number plates](https://medusa.fit.vutbr.cz/traffic/download/513/) which are 652 images of cropped license plates with a csv containing annotation.


I have resized all images to (650, 250, 3)

## Generate tfrecords
Once we have our images of equal sizes in a different directory, we can begin using those images to generate tfrecords that we will use to train our dataset. The script to generate tfrecords can be found [here](https://github.com/NanoNets/number-plate-detection/blob/master/src/get_tf_records.py). The train tfrecords and test tfrecords along with the charset label mapping are stored in the directory- ```python/datasets/data/number_plates```

The dataset has to be in the FSNS dataset format. For this, your test and train tfrecords along with the charset labels text file are placed inside a folder named ```data/number_plates``` inside the ```datasets``` directory. The charset-labels.txt file used for this  project can be found [here](https://github.com/codeaway23/models/blob/master/research/attention_ocr/python/datasets/data/number_plates/charset-labels.txt)



## Setting our Attention-OCR up
Once we have our tfrecords and charset labels stored in the required directory, we need to write a dataset config script that will help us split our data into train and test for the attention OCR training script to process.

Refer the file ```number_plates.py``` in ```python/datasets``` for the same

## Training the model
The file named ```common_flags.py``` is used to specify the location where you want to log your training.

We are done with the preprocessing steps.

To try out this Attention OCR model for number plate recognition, simply download the project

Go to inside the ```python``` directory 
and then run

```bash
python train.py --dataset_name=number_plates --max_number_of_steps=6000
```

Once you train the model, you'll see ```aocr-logs``` where training logs and the model checkpoint is stored.

## Evaluating the model

Run the following command from terminal to evaluate the  model.

```bash
python eval.py --dataset_name='number_plates'
```

## Get predictions
Inside ```python``` directory run the following command on your shell.

```bash
python demo_inference.py --dataset_name=number_plates \n 
--batch_size=8 --checkpoint=aocr-logs/model.ckpt-6000 \n
--image_path_pattern='path_to_the_imagefile.jpg'
```
